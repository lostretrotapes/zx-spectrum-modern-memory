# ZX Spectrum 48k Moderm Memory Replacement Boards

A work in progress to create a PCB to replace the upper and lower RAM chips in the 48k Spectrum with a modern replacement

## BOM

- SRAM 256K 32K x 8 [Mouser](https://www.mouser.co.uk/ProductDetail/Alliance-Memory/CY62256NLL-55SNXI?qs=byeeYqUIh0Pp3ev9t84NaQ%3D%3D)
- Flip-Flops 74HCT574D [Mouser](https://www.mouser.co.uk/ProductDetail/Nexperia/74HCT574D653?qs=me8TqzrmIYV85vlD5SZpSA%3D%3D)
- Logic Gates Quadruple 2-Input Positive-NAND gates SN74ACT00NSR [Mouser](https://www.mouser.co.uk/ProductDetail/Texas-Instruments/SN74ACT00NSR?qs=pdM4geFiyZQ6dUA3Pnq9Sg%3D%3D)

## References

- https://sindik.at/data/vram/
- https://www.evolutional.co.uk/post/zxspectrum-4116-ram-board/

## Technical

 Technical challenge in interfacing different types of memory chips - DRAM (Dynamic Random Access Memory) and SRAM (Static Random Access Memory) - and a solution using additional components.

### Address Line Multiplexing in DRAM
The 4116 DRAM chip uses a process called 'multiplexing' for its address lines. This means it uses the same set of address lines (A0-A6) first for 'row' address and then for 'column' address. To switch between these two, it uses two signals: !RAS (Row Address Strobe) and !CAS (Column Address Strobe). You set the row address, activate !RAS, wait, set the column address, activate !CAS, and wait again.

### SRAM's Different Addressing Method
The SRAM chip, however, does not multiplex its address lines. It expects all address lines to be provided at once, not in two parts like DRAM.

### Solution
Using a Flip Flop: To make the SRAM work like DRAM in terms of addressing, a flip flop can be used. A flip flop is a basic memory circuit that can hold a set of binary values (0s and 1s). Here, a flip flop with at least 7 inputs/outputs is needed. The idea is to first set the flip flop with the row address using the !RAS signal, then use its output as the 'row' lines for the SRAM.

### Choosing the Right Flip Flop - 74HC374
The 74HC374 is an octal (8-bit) flip flop and fits the requirement. It can take in 8 bits of data and store them until needed.

### Addressing the Active High/Low Issue
The 74HC374 has a small mismatch with the DRAM’s !RAS signal. The !RAS signal is active low (it activates on a 0), but the chip select on the 74HC374 is active high (activates on a 1).

### Incorporating an Inverter - 74HC04
To resolve this, an inverter (74HC04) is used. This inverter will flip the signal from active low to active high, making it compatible with the 74HC374. The 74HC04 has 6 inverters, more than needed for this task, but it's necessary to make the connection work.

In simple terms, the above describes how to connect a DRAM and an SRAM chip, which handle addressing differently, by using a flip flop and an inverter to match their signal requirements.