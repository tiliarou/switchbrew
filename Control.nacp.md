Roughly equivalent to non-gfx areas of 3DS SMDH. Also includes a string
for the display-version of this title. All strings are UTF-8, unlike
SMDH which uses UTF-16.

Total size is 0x4000-bytes.

# Structure

| Offset | Size   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| 0x0    | 0x2400 | Language entries                                |
| 0x2400 | 0xC00  | Normally all-zero? Maybe more language entries? |
| 0x3000 | ...    | ...                                             |

## Language Entry

Total size is 0x300-bytes.

| Offset | Size  | Description                  |
| ------ | ----- | ---------------------------- |
| 0x0    | 0x200 | Application name string      |
| 0x200  | 0x100 | Application developer string |