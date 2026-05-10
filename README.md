# ZX-Spectrum-plus2A-fdd-interface

Hardware interface to connect standard 3.5" floppy disk drives (or Gotek emulators) to the ZX Spectrum +2A/2B using the native +3DOS.





markdown## 🛠️ Hardware Specifications





| Component | Specification | Description |

| :--- | :--- | :--- |

| \*\*FDC Chip\*\* | NEC μPD765A / Intel 8272A | Industry standard floppy disk controller (same as ZX Spectrum +3) |

| \*\*Data Separator\*\* | SED9420 / FDC9216 (or discrete discrete logic) | Ensures reliable data recovery from the drive read-head signal |

| \*\*Address Decoding\*\* | Full $+1FFD$ and $+7FFD$ decoding | Maps perfectly to the $+2A/+2B$ hardware memory/banking ports |

| \*\*Bus Loading\*\* | Buffered data/address bus | Uses 74HCT/LS buffers to protect 







\## 🔧 PC 3.5" Floppy Drive Modification (Shugart / READY Mod)



Standard PC 3.5-inch floppy drives (Sony, Samsung, Mitsumi, Panasonic) are hardwired to standard PC specifications. To make them work with the μPD765 controller, you must perform two hardware modifications on the drive's PCB:

1\. \*\*Change Drive ID from DS1 to DS0\*\* (Spectrum expects Drive A: to be DS0).

2\. \*\*Route the READY signal to Pin 34\*\* (PC drives put `Disk Change` on Pin 34).



Below are instructions for the most common drive models found today.



\---



\### 📸 Sony MPF920 (Most Common)

1\. \*\*Drive ID (DS0)\*\*: Locate the jumper pads labeled `JC30` / `JC31` (or `DS0` / `DS1`) near the data connector. Remove the zero-ohm resistor (or solder bridge) from `DS1` and solder it across the `DS0` pads.

2\. \*\*READY Signal\*\*: 

&#x20;  \* Locate the trace going to \*\*Pin 34\*\* of the IDC connector. Cut this trace to disconnect the `Disk Change` signal.

&#x20;  \* Locate \*\*Pin 5\*\* of the main drive controller chip (or find a pad labeled `RDY`).

&#x20;  \* Solder a thin kynar wire from the `RDY` pad directly to \*\*Pin 34\*\* of the interface connector.



\---



\### 📸 Samsung SFD-321B

1\. \*\*Drive ID (DS0)\*\*: Locate the solder pads labeled `DC0` / `DC1` (or `S0` / `S1`). Move the SMD resistor/bridge from position `1` to position `0`.

2\. \*\*READY Signal\*\*:

&#x20;  \* Near the 34-pin connector, look for configuration pads labeled `DC` (Disk Change) and `RDY` (Ready).

&#x20;  \* By default, a bridge connects the main line to `DC`. Desolder this bridge and move it to the `RDY` position. (No wire cutting required on most revisions!).



\---



\### ⚙️ Alternative: Gotek Floppy Emulator Configuration

If you are using a \*\*Gotek\*\* drive instead of a real mechanical drive, no soldering is required. Flash the drive with \[FlashFloppy firmware](https://github.com) and update your `FF.CFG` configuration file on the USB drive with the following lines:

```ini

interface = shugart

host = dec-shugart

pin34 = rdy

```

\*\*\*



