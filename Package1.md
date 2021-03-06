Present in the firmware package titles (0100000000000819,
010000000000081A, 010000000000081B and 010000000000081C) and installed
into eMMC storage's [boot partitions 0 and
1](Flash%20Filesystem#Boot%20Partitions.md##Boot_Partitions "wikilink"),
"package1" contains the first Switch bootloader ("Package1ldr") to run
under the NVIDIA boot processor (an ARM7TDMI called "BPMP", "BPMP-Lite",
"AVP" or "COP"), as well as the actual encrypted package1 ("PK11") blob
containing the second Switch Bootloader and TrustZone code.

The boot ROM validates, copies to IRAM and executes this package by
parsing it's information block from the [BCT](BCT.md "wikilink").

# Format

This package is distributed as a plaintext initial bootloader
(package1ldr) and a secondary encrypted blob ("PK11"). Execution starts
at plaintext package1ldr which will set up hardware, generate keys and
decrypt the next stage.

## Package1ldr

The code for this stage is stored in plaintext inside the package. By
looking into the BCT's bootloader0\_info (normal) or bootloader1\_info
(safe mode), the boot ROM starts executing this stage at address
0x40010020 in IRAM (0x40010040 for
4.0.0+).

### Header

| Offset | Size | Description                                                        |
| ------ | ---- | ------------------------------------------------------------------ |
| 0x0    | 0x4  | Package1ldr hash (first four bytes of SHA256(package1ldr)).        |
| 0x4    | 0x4  | Secure Monitor hash (first four bytes of SHA256(secure\_monitor)). |
| 0x8    | 0x4  | NX Bootloader hash (first four bytes of SHA256(nx\_bootloader)).   |
| 0xC    | 0x4  | Build ID                                                           |
| 0x10   | 0xE  | Build Timestamp (yyyyMMddHHmmss)                                   |
| 0x1E   | 0x2  | Version                                                            |

### Initialization

The stack pointer is set.

``` c
 // Set the stack pointer
 *(u32 *)sp = 0x40008000;
 
 // Jump to main
 bootloader_main();
 
 // Infinite loop
 deadlock();
```

### Main

From firmware versions 1.0.0 to 6.1.0, the bootloader poisons the
exception vectors, cleans up memory (.bss and init\_array), sets up
hardware devices (including the security engine and fuses), does all the
necessary checks, generates keys and finally decrypts and executes the
next stage.

``` c
 // Poison all exception vectors
 *(u32 *)0x6000F200 = panic();
 *(u32 *)0x6000F204 = panic();
 *(u32 *)0x6000F208 = panic();
 *(u32 *)0x6000F20C = panic();
 *(u32 *)0x6000F210 = panic();
 *(u32 *)0x6000F214 = panic();
 *(u32 *)0x6000F218 = panic();
 *(u32 *)0x6000F21C = panic();
 
 u32 bss_addr_end = bss_addr_start;
 u32 bss_offset = 0;
 u32 bss_size = bss_addr_end - bss_addr_start;
 
 // Clear .bss region
 // Never happens due to bss_size being set to 0
 while (bss_offset < bss_size)
 {
    *(u32 *)bss_addr_start + bss_offset = 0;
    bss_offset += 0x04;
 }
 
 u32 init_array_addr_end = init_array_addr_start;
 u32 init_array_offset = init_array_addr_start;
 
 // Call init methods
 // Never happens due to init_array_addr_end being set to init_array_addr_start
 while (init_array_offset < init_array_addr_end)
 {
    u32 init_method_offset = *(u32 *)init_array_offset;
 
    call_init_method(init_method_offset + init_array_offset);
    init_array_offset += 0x04;
 }
 
 // Setup I2S1, I2S2, I2S3, I2S4, DISPLAY and VIC
 mbist_workaround();
 
 // Program the SE clock and resets
 // Uses RST_DEVICES_V, CLK_OUT_ENB_V, CLK_SOURCE_SE and CLK_V_SE
 enable_se_clkrst();
 
 // Set MISC_CLK_ENB
 // This makes fuse registers visible
 enable_misc_clk(0x01);
 
 // Read FUSE_SKU_INFO and compare with 0x83
 check_sku();
 
 // Check configuration fuses
 check_config_fuses();
 
 u32 bct_iram_addr = 0x40000000;
 
 // Check bootloader version from BCT
 check_bootloader_ver(bct_iram_addr);
 
 // Check anti-downgrade fuses
 check_downgrade();
 
 // Set FUSE_DIS_PGM
 // Disables fuse programming until next reboot
 disable_fuse_pgm();
 
 // Setup memory controllers
 enable_mem_ctl();
 
 // Setup the security engine's address
 set_se_addr(0x70012000);
 
 // Check SE global config
 check_se_status();
 
 // Generate keys
 keygen(bct_iram_addr);
 
 u32 pk11_blob_addr = 0x40013FE0;
 
 // Decrypt the PK11 blob and get the next stage's entrypoint
 nx_boot_addr = decrypt_pk11_blob(pk11_blob_addr);
 
 u32 nx_boot_sp = 0x40007000;
 
 // Set the stack pointer and jump to a stub responsible
 // for cleaning up and branching into the next stage
 exec_nx_boot_stub(nx_boot_addr, nx_boot_stub_addr, nx_boot_sp);
 
 return;
```

Starting with firmware version 6.2.0, the bootloader maintains most of
its design, but passes execution to a [TSEC](TSEC.md "wikilink") payload
and is left in an infinite loop.

``` c
 // Poison all exception vectors
 *(u32 *)0x6000F200 = panic();
 *(u32 *)0x6000F204 = panic();
 *(u32 *)0x6000F208 = panic();
 *(u32 *)0x6000F20C = panic();
 *(u32 *)0x6000F210 = panic();
 *(u32 *)0x6000F214 = panic();
 *(u32 *)0x6000F218 = panic();
 *(u32 *)0x6000F21C = panic();
 
 u32 bss_addr_end = bss_addr_start;
 u32 bss_offset = 0;
 u32 bss_size = bss_addr_end - bss_addr_start;
 
 // Clear .bss region
 // Never happens due to bss_size being set to 0
 while (bss_offset < bss_size)
 {
    *(u32 *)bss_addr_start + bss_offset = 0;
    bss_offset += 0x04;
 }
 
 u32 init_array_addr_end = init_array_addr_start;
 u32 init_array_offset = init_array_addr_start;
 
 // Call init methods
 // Never happens due to init_array_addr_end being set to init_array_addr_start
 while (init_array_offset < init_array_addr_end)
 {
    u32 init_method_offset = *(u32 *)init_array_offset;
 
    call_init_method(init_method_offset + init_array_offset);
    init_array_offset += 0x04;
 }
 
 // Setup I2S1, I2S2, I2S3, I2S4, DISPLAY and VIC
 mbist_workaround();
 
 // Program the SE clock and resets
 // Uses RST_DEVICES_V, CLK_OUT_ENB_V, CLK_SOURCE_SE and CLK_V_SE
 enable_se_clkrst();
 
 // Set MISC_CLK_ENB
 // This makes fuse registers visible
 enable_misc_clk(0x01);
 
 // Setup the security engine's address
 set_se_addr(0x70012000);
 
 // Check SE global config
 check_se_status();
 
 // Read FUSE_SKU_INFO and compare with 0x83
 check_sku();
 
 // Check configuration fuses
 check_config_fuses();
 
 u32 bct_iram_addr = 0x40000000;
 
 // Check bootloader version from BCT
 check_bootloader_ver(bct_iram_addr);
 
 // Check anti-downgrade fuses
 check_downgrade();
 
 // Setup memory controllers
 enable_mem_ctl();
  
 // Clear SYS_CLK_DIVISOR
 *(u32 *)CLK_SOURCE_SYS = 0;
 
 // Place I2C5 in reset
 u32 rst_dev_h_val = *(u32 *)RST_DEVICES_H;
 rst_dev_h_val &= ~(0x8000);
 rst_dev_h_val |= 0x8000;
 *(u32 *)RST_DEVICES_H = rst_dev_h_val;
 
 // Program the HOST1X clock and resets
 // Uses RST_DEVICES_L, CLK_OUT_ENB_L, CLK_SOURCE_HOST1X and CLK_L_HOST1X
 enable_host1x_clkrst();
 
 // Program the TSEC clock and resets
 // Uses RST_DEVICES_U, CLK_OUT_ENB_U, CLK_SOURCE_TSEC and CLK_U_TSEC
 enable_tsec_clkrst();
 
 // Program the SOR_SAFE clock and resets
 // Uses RST_DEVICES_Y, CLK_OUT_ENB_Y and CLK_Y_SOR_SAFE
 enable_sor_safe_clkrst();
 
 // Program the SOR0 clock and resets
 // Uses RST_DEVICES_X, CLK_OUT_ENB_X and CLK_X_SOR0
 enable_sor0_clkrst();
 
 // Program the SOR1 clock and resets
 // Uses RST_DEVICES_X, CLK_OUT_ENB_X, CLK_SOURCE_SOR1 and CLK_X_SOR1
 enable_sor1_clkrst();
 
 // Program the KFUSE clock resets
 // Uses RST_DEVICES_H, CLK_OUT_ENB_H and CLK_H_KFUSE
 enable_kfuse_clkrst();
 
 // Clear the Falcon DMA control register
 *(u32 *)FALCON_DMACTL = 0;
 
 // Enable Falcon IRQs
 *(u32 *)FALCON_IRQMSET = 0xFFF2;
 
 // Enable Falcon IRQs
 *(u32 *)FALCON_IRQDEST = 0xFFF0;
 
 // Enable Falcon interfaces
 *(u32 *)FALCON_ITFEN = 0x03;
 
 // Wait for Falcon's DMA engine to be idle
 wait_flcn_dma_idle();
 
 // Set DMA transfer base address to 0x40010E00>> 0x08
 *(u32 *)FALCON_DMATRFBASE = 0x40010E;
 
 u32 trf_mode = 0;     // A value of 0 sets FALCON_DMATRFCMD_IMEM
 u32 dst_offset = 0;
 u32 src_offset = 0;
 
 // Load code into Falcon (0x100 bytes at a time)
 while (src_offset < 0x2900)
 {
    flcn_load_firm(trf_mode, src_offset, dst_offset);
    src_offset += 0x100;
    dst_offset += 0x100;
 }
 
 // Set magic value in host1x scratch space
 *(u32 *)0x50003300 = 0x34C2E1DA;
 
 // Clear Falcon scratch1 MMIO
 *(u32 *)FALCON_SCRATCH1 = 0;
 
 // Set Falcon boot key version in scratch0 MMIO
 *(u32 *)FALCON_SCRATCH0 = 0x01;
 
 // Set Falcon's boot vector address
 *(u32 *)FALCON_BOOTVEC = 0;
 
 // Signal Falcon's CPU
 *(u32 *)FALCON_CPUCTL = 0x02;
 
 // Infinite loop
 deadlock();
```

#### Panic

If a panic occurs, all sensitive memory contents are cleared, the
security engine and fuse programming are disabled and the boot processor
is left in a halted state.

``` c
 // Clear all stack contents
 clear_stack();
 
 // Terminate the security engine
 disable_se();
 
 // Set FUSE_DIS_PGM
 // Disables fuse programming until next reboot
 disable_fuse_pgm();
 
 // Clear temporary key storage memory
 clear_mem();
 
 // Clear the PK11 blob from memory
 clear_pk11_blob();
 
 // Halt the boot processor
 while (true)
    *(u32 *)FLOW_CTLR_HALT_COP_EVENTS = (FLOW_MODE_STOP | HALT_COP_EVENT_JTAG);
```

#### Anti-downgrade

See
[Anti-downgrade](Fuses#Anti-downgrade.md##Anti-downgrade "wikilink").

#### Memory controllers

After disabling fuse programming, the bootloader configures the EMC and
MEM/MC. It additionally disables QSPI resets and programs a special
aperture designed for AHB redirected access to IRAM.

``` c
 
 // Initialize EMC's clock source
 u32 emc_clk_src_val = *(u32 *)PERIPH_CLK_SOURCE_EMC;
 *(u32 *)PERIPH_CLK_SOURCE_EMC = (emc_clk_src_val | 0x40000000);
 
 // Enable CLK_H_EMC
 u32 clk_out_enb_h_val = *(u32 *)CLK_OUT_ENB_SET_H;
 clk_out_enb_h_val &= ~(0x2000000);
 clk_out_enb_h_val |= 0x2000000;
 *(u32 *)CLK_OUT_ENB_SET_H = clk_out_enb_h_val;
 
 // Enable CLK_H_MEM
 clk_out_enb_h_val = *(u32 *)CLK_OUT_ENB_SET_H;
 clk_out_enb_h_val &= ~(0x01);
 clk_out_enb_h_val |= 0x01;
 *(u32 *)CLK_OUT_ENB_SET_H = clk_out_enb_h_val;
 
 // Enable CLK_X_EMC_DLL
 u32 clk_out_enb_x_val = *(u32 *)CLK_OUT_ENB_SET_X;
 clk_out_enb_x_val &= ~(0x4000);
 clk_out_enb_x_val |= 0x4000;
 *(u32 *)CLK_OUT_ENB_SET_X = clk_out_enb_x_val;
 
 // Enable RST_H_EMC and RST_H_MEM
 *(u32 *)RST_DEVICES_SET_H = 0x2000001;
 
 // Wait a while
 mdelay(0x05);
 
 // Disable RST_Y_QSPI
 u32 rst_clr_y_val = *(u32 *)RST_DEVICES_CLR_Y;
 rst_clr_y_val &= ~(0x80000);
 rst_clr_y_val |= 0x80000;
 *(u32 *)RST_DEVICES_CLR_Y = rst_clr_y_val;
 
 // Refresh MC_IRAM_REG_CTRL
 // Should be set to 0 (MC_ENABLE_IRAM_CFG_WRITES)
 u32 mc_iram_reg_ctrl_val = *(u32 *)MC_IRAM_REG_CTRL;
 *(u32 *)MC_IRAM_REG_CTRL = mc_iram_reg_ctrl_val;
 
 // Set base and top addresses for AHB redirected IRAM path
 // This allows devices like the GPU to access this range
 *(u32 *)MC_IRAM_BOM = 0x40000000;
 *(u32 *)MC_IRAM_TOM = 0x4003F000;
 
 // Read back MC_IRAM_REG_CTRL
 mc_iram_reg_ctrl_val = *(u32 *)MC_IRAM_REG_CTRL;
    
 return mc_iram_reg_ctrl_val;
```

#### Key generation

After the security engine is ready and before decrypting the next stage,
the bootloader initializes and generates several keys into hardware
keyslots. For more details on the Switch's cryptosystem, please see
[this page](Cryptosystem.md "wikilink").

\[6.2.0+\] The key generation process was moved into an encrypted
[TSEC](TSEC.md "wikilink") payload.

##### Selection

Depending on
[FUSE\_RESERVED\_ODM4](Fuses#FUSE%20RESERVED%20ODM4.md##FUSE_RESERVED_ODM4 "wikilink")
and
[FUSE\_SPARE\_BIT\_5](Fuses#FUSE%20SPARE%20BIT%205.md##FUSE_SPARE_BIT_5 "wikilink")
different static seeds are selected for key generation.

``` c
 // Initialize keyslots 0x0C and 0x0D as readable
 init_keyslot(0x0C, 0x15);
 init_keyslot(0x0D, 0x15);
 
 // Find the BCT's data address from IRAM header
 u32 bct_data_addr = *(u32 *)bct_imem_addr + 0x4C;
 u32 bct_customer_data_addr = *(u32 *)bct_data_addr + 0x450;
 
 // Wrapper to get unit type from FUSE_RESERVED_ODM4
 // This tells if the device is retail or debug
 bool is_retail = is_unit_retail();
 
 u32 master_static_seed_addr = 0;
 u32 master_static_seed_size = 0;
 
 if (is_retail)
 {
    // Read FUSE_SPARE_BIT_5
    // This tells which master key to use
    u32 master_key_ver = read_fuse_spare_bit_5();
 
    // Invalid for retail
    if (!master_key_ver)
       panic();
    else
    {
       master_static_seed_addr = static_seed1_addr;
       master_static_seed_size = 0x10;
 
       // Generate retail keys
       generate_retail_keys(bct_customer_data_addr, static_seed_addr, static_seed_size);
    }
 }
 else
 {
    // Read FUSE_SPARE_BIT_5
    // This tells which master key to use
    u32 master_key_ver = read_fuse_spare_bit_5();
 
    // Use debug key set
    if (!master_key_ver)
    {
       // Read the first byte of the BCT RSA PSS signature
       u8 rsa_pss_1_byte = *(u8 *)bct_data_addr + 0x210;
 
       if (rsa_pss_1_byte == 0x11)
       {
          master_static_seed_addr = static_seed6_addr;
          master_static_seed_size = 0x10;
       }
       else                           
       {
          master_static_seed_addr = static_seed7_addr;
          master_static_seed_size = 0x10;
       }
 
       // Generate debug keys
       generate_debug_keys(static_seed_addr, static_seed_size);
    }
    else
    {
       // Read the first byte of the BCT RSA PSS signature
       u8 rsa_pss_1_byte = *(u8 *)bct_data_addr + 0x210;
 
       if (rsa_pss_1_byte == 0x4F)      // Different key as in retail mode
       {
          master_static_seed_addr = static_seed0_addr;
          master_static_seed_size = 0x10;
       }
       else                                 // Same key as in retail mode
       {
          master_static_seed_addr = static_seed1_addr;
          master_static_seed_size = 0x10;
       }
 
       // Generate retail keys
       generate_retail_keys(bct_customer_data_addr, master_static_seed_addr, master_static_seed_size);
    }
 }
 
 // Initialize keyslots 0x0C and 0x0D as unreadable
 init_keyslot(0x0C, 0xFF);
 init_keyslot(0x0D, 0xFF);
 
 return;
```

##### generate\_retail\_keys

In order to generate retail keys, the bootloader starts by initializing
TSEC and grabbing it's [device
key](TSEC#TSEC%20key%20generation.md##TSEC_key_generation "wikilink").
Using static seeds and the SBK, the keyblob injected into the BCT's
[customer\_data](BCT#customer%20data.md##customer_data "wikilink") is
validated and decrypted. The resulting keys will then be used to
generate the master static key and the master device key.

See the pseudocode bellow for the detailed process.

``` c
 u32 in_addr = 0;
 u32 in_size = 0;
 u32 out_addr = 0;
 u32 out_size = 0;
 u32 keyslot = 0;
 u32 keyslot_dst = 0;
 
 // Get the TSEC device key
 tsec_get_device_key(tsec_device_key_addr, 0x10);
 
 // Install the TSEC device key into keyslot 0x0D
 set_keyslot_data(0x0D, tsec_device_key_addr, 0x10);
 
 in_addr = static_seed2_addr;
 in_size = 0x10;
 out_addr = keyblob_device_key_addr;
 out_size = 0x10;
 keyslot = 0x0D;
 
 // Use the tsec_device_key (keyslot 0x0D) to decrypt the static_seed2
 // This generates the keyblob_device_key
 aes_ecb_decrypt(out_addr, out_size, keyslot, in_addr, in_size);
 
 in_addr = keyblob_device_key_addr;
 in_size = 0x10;
 keyslot = 0x0E;
 keyslot_dst = 0x0D;
 
 // Use SBK (keyslot 0x0E) to further decrypt the
 // keyblob_device_key and install it into keyslot 0x0D
 // This will generate the keyblob_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 // Clear SBK and SSK keyslots
 clear_keyslot(0x0E);
 clear_keyslot(0x0F);
 
 in_addr = static_seed4_addr;
 in_size = 0x10;
 keyslot = 0x0D;
 keyslot_dst = 0x0B;
 
 // Use keyblob_key (keyslot 0x0D) to decrypt the
 // static_seed4_addr and install it to keyslot 0x0B
 // This will generate the bct_mac_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 in_addr = bct_customer_data_addr + 0x10;
 in_size = 0xA0;
 out_addr = mac_addr;
 out_size = 0x10;
 keyslot = 0x0B;
 
 // Use the bct_mac_key (keyslot 0x0B) to generate
 // CMAC over bct_customer_data_addr + 0x10
 aes_cmac(out_addr, out_size, keyslot, in_addr, in_size);
 
 // Compare the generated hash with the first
 // 0x10 bytes of bct_customer_data
 bool match = safe_memcmp(mac_addr, bct_customer_data_addr, 0x10);
 
 // Hashes don't match
 if (!match)
    panic();
 
 in_addr = bct_customer_data_addr + 0x20;
 in_size = 0x90;
 ctr_addr = bct_customer_data_addr + 0x10;
 ctr_size = 0x10;
 out_addr = dec_payload_addr;
 out_size = 0x90;
 keyslot = 0x0D;
 
 // AES-CTR decrypt
 // Use the keyblob_key (keyslot 0x0D) to decrypt bct_customer_data_addr + 0x20
 // using bct_customer_data_addr + 0x10 as CTR
 aes_ctr_decrypt(out_addr, out_size, keyslot, in_addr, in_size, ctr_addr, ctr_size);
 
 // Install the last decrypted keyblob key into keyslot 0x0B
 // This is the pk11_key
 set_keyslot_data(0x0B, dec_payload_addr + 0x80, 0x10);
 
 // Install the first decrypted keyblob key into keyslot 0x0C
 // This is the master_static_kek
 set_keyslot_data(0x0C, dec_payload_addr, 0x10);
 
 // Clear out the decrypted data
 memclear(dec_payload_addr, 0x90);
 
 in_addr = master_static_seed_addr;
 in_size = master_static_seed_size;
 keyslot = 0x0C;
 keyslot_dst = 0x0C;
 
 // Use the master_static_kek (keyslot 0x0C) to decrypt
 // master_static_seed and install it into keyslot 0x0C
 // This will generate the master_static_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 in_addr = static_seed3_addr;
 in_size = 0x10;
 keyslot = 0x0D;
 keyslot_dst = 0x0D;
 
 // Use keyblob_key (keyslot 0x0D) to decrypt
 // static_seed3_addr and install it into keyslot 0x0D
 // This will generate the master_device_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 return;
```

##### generate\_debug\_keys

In order to generate debug keys, the bootloader only uses static seeds,
the SBK and the SSK.

See the pseudocode bellow for the detailed process.

``` c
 u32 in_addr = 0;
 u32 in_size = 0;
 u32 keyslot = 0;
 u32 keyslot_dst = 0;
 
 in_addr = static_seed8_addr;
 in_size = 0x10;
 keyslot = 0x0E;
 keyslot_dst = 0x0B;
 
 // Use SBK (keyslot 0x0E) to decrypt the
 // static_seed8 and install it to keyslot 0x0B
 // This will generate debug_pk11_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 in_addr = static_seed5_addr;
 in_size = 0x10;
 keyslot = 0x0E;
 keyslot_dst = 0x0C;
 
 // Use SBK (keyslot 0x0E) to decrypt the
 // static_seed5 and install it to keyslot 0x0C
 // This will generate debug_master_static_kek
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 in_addr = static_seed9_addr;
 in_size = 0x10;
 keyslot = 0x0F;
 keyslot_dst = 0x0D;
 
 // Use SSK (keyslot 0x0F) to decrypt the
 // static_seed9 and install it to keyslot 0x0D
 // This will generate debug_keyblob_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 // Clear SBK and SSK keyslots
 clear_keyslot(0x0E);
 clear_keyslot(0x0F);
 
 in_addr = master_static_seed_addr;
 in_size = master_static_seed_size;
 keyslot = 0x0C;
 keyslot_dst = 0x0C;
 
 // Use the debug_master_static_kek (keyslot 0x0C) to decrypt the
 // master_static_seed and install it to keyslot 0x0C
 // This will generate the master_static_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 in_addr = static_seed3_addr;
 in_size = 0x10;
 keyslot = 0x0D;
 keyslot_dst = 0x0D;
 
 // Use debug_keyblob_key (keyslot 0x0D) to decrypt the
 // static_seed3 and install it to keyslot 0x0D
 // This will generate the master_device_key
 decrypt_keyslot(keyslot_dst, keyslot, in_addr, in_size);
 
 return;
```

## Package1 (PK11)

This blob is stored encrypted inside the package and is decrypted by
package1ldr.

### Encryption

The encrypted blob is prepended with it's CTR and total image size.
After checking the image's size against an hardcoded value (can change
on firmware updates), the image is AES-CTR decrypted and the keyslot
used for decryption is immediately cleared.

``` c
 // Maximum encrypted blob's size on firmware version 1.0.0
 u32 max_pk11_enc_blob_size = 0x29000;
 
 u32 pk11_enc_blob_size = *(u32 *)pk11_blob_addr;
 u32 pk11_enc_blob_ctr_addr = pk11_blob_addr + 0x10; 
 u32 pk11_enc_blob_addr = pk11_blob_addr + 0x20;
 
 // Validate the encrypted blob's size
 if (pk11_enc_blob_size > max_pk11_enc_blob_size)
    panic();
 
 u32 in_addr = pk11_enc_blob_addr;
 u32 in_size = pk11_enc_blob_size;
 u32 ctr_addr = pk11_enc_blob_ctr_addr;
 u32 ctr_size = 0x10;
 u32 out_addr = pk11_dec_blob_addr;
 u32 out_size = pk11_dec_blob_size;
 u32 keyslot = 0x0B;
 
 // AES-CTR decrypt
 // Use the pk11_key (keyslot 0x0B) to decrypt the blob in place
 aes_ctr_decrypt(out_addr, out_size, keyslot, in_addr, in_size, ctr_addr, ctr_size);
 
 // Clear pk11_key keyslot
 clear_keyslot(0x0B);
 
 // Validate the decrypted blob
 // Checks the "PK11" magic and some pk11 header fields
 bool is_valid = check_pk11_header(pk11_dec_blob_addr, pk11_dec_blob_size);
 
 // Invalid PK11 image
 if (!is_valid)
    panic();
 
 u32 pk11_header_size = 0x20;
 u32 pk11_sec1_offset = *(u32 *)pk11_dec_blob_addr + 0x14;
 u32 pk11_sec2_size = *(u32 *)pk11_dec_blob_addr + 0x18;
 
 // Calculate NX bootloader's entrypoint
 u32 nx_boot_addr = (pk11_dec_blob_addr + pk11_header_size + pk11_sec1_offset + pk11_sec2_size);
        
 return nx_boot_addr;
```

### Header

When decrypted, the blob is encapsulated in the following header.

| Offset | Size | Description      |
| ------ | ---- | ---------------- |
| 0x0    | 4    | Magic "PK11"     |
| 0x4    | 4    | Section 0 size   |
| 0x8    | 4    | Section 0 offset |
| 0xC    | 4    | Unknown          |
| 0x10   | 4    | Section 1 size   |
| 0x14   | 4    | Section 1 offset |
| 0x18   | 4    | Section 2 size   |
| 0x1C   | 4    | Section 2 offset |

What each section is used for may vary per system-version.

### Section 0

This section contains the warmboot binary.

### Section 1

This section contains the NX bootloader, which is run after the initial
bootloader in package1.

### Section 2

This section contains the Secure Monitor binary.

# Changelog

## 2.0.0

  - The encrypted binaries' order and calculation for next stage's
    entrypoint was changed.

Old layout (before
2.0.0):

`1.- PK11 header`  
`2.- Secure Monitor blob`  
`3.- NX bootloader blob`  
`4.- Warmboot blob`  
  
`NX bootloader entrypoint is calculated as:`  
`0x40013FE0 + 0x20 + 0x20 + NX bootloader blob's offset + Secure Monitor blob's size`

New layout
(2.0.0+):

`1.- PK11 header`  
`2.- Warmboot blob`  
`3.- NX bootloader blob`  
`4.- Secure Monitor blob`  
  
`NX bootloader entrypoint is calculated as:`  
`0x40013FE0 + 0x20 + 0x20 + NX bootloader blob's offset + Warmboot blob's size`

  - Some AES-ECB decryption related code was refactored.

## 3.0.0

  - The functions set\_se\_addr() and check\_se\_status() are now called
    right after enabling the security engine clocks and resets.

See
[Switch\_System\_Flaws\#Stage\_1\_Bootloader](Switch%20System%20Flaws#Stage%201%20Bootloader.md##Stage_1_Bootloader "wikilink").

  - Keyslot 0x0A is now used instead of keyslot 0x0D for generating the
    master\_device\_key.
