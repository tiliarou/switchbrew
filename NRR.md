The Switch uses the NRR file format to verify [NRO](NRO.md "wikilink")
at load time. These files contain hashes of each NRO that is allowed to
be loaded by the program. An NRO's SHA-256 hash must match any of the
hashes in the hash table. NRRs are signed with
RSASSA-PSS-2048/SHA-256.

| Offset | Size        | Description                                                               |
| ------ | ----------- | ------------------------------------------------------------------------- |
| 0x00   | 4           | “NRR0” magic                                                              |
| 0x04   | 28          | Unknown (0)                                                               |
| 0x20   | 8           | Title ID mask?                                                            |
| 0x28   | 8           | Title ID pattern?                                                         |
| 0x30   | 256         | Modulus for verifying the second signature                                |
| 0x130  | 256         | First signature signed by a Nintendo key, over the above contents         |
| 0x230  | 256         | Second signature verifiable with the above key, over the rest of the file |
| 0x330  | 8           | Title ID                                                                  |
| 0x338  | 4           | Size                                                                      |
| 0x33C  | 4           | Unknown (0)                                                               |
| 0x340  | 4           | Hash offset (0x350)                                                       |
| 0x344  | 4           | Hash count                                                                |
| 0x348  | 8           | Unknown (0)                                                               |
| 0x350  | 32 \* count | NRO hashes (SHA-256)                                                      |

[Category:File formats](Category:File_formats "wikilink")
