# VLSI Design
ijj
Coursework and standard-cell design exercises targeting the SKY130 PDK. The
repository contains SPICE simulation, Magic layout, LVS/parasitic extraction,
cell characterization, and Verilog verification collateral.

## Repository layout

| Path | Contents |
| --- | --- |
| [`Assignment_1`](Assignment_1) | SPICE netlists and the Assignment 1 report for INVX1 and INVX2 analysis. |
| [`Assignment_2`](Assignment_2) | Magic layouts, schematic/layout/PEX netlists, and INVX1 CharLib configuration. |
| [`Project_1`](Project_1) | AND2 and positive-edge DFF standard-cell project files. |

## Assignment 1: SPICE Netlist for INVX1 and INVX2
1. INVX1 Transient and Static Analysis in ngspice
2. INVX1 Transient and Static Analysis in ngspice

## Assignment 2: Layout and Parasitics
1. INVX1 :
   > Create Layout in Magic
   > Export SPICE netlist and 
   ``` cli
   extract all
   ext2spice lvs
   ext2spice -d -o invx1_layout.spice
   ```
   > Compare with Assignment_1 netlist
   ``` cli
   netgen -batch lvs "invx1_layout.spice INVX1" "invx1.ckt INVX1" /foss/pdks/sky130A/libs.tech/netgen/sky130A_steup.tcl
   ```
   > Export Parasitics
   ``` cli
   extract all
   ext2spice scale off
   ext2spice cthresh 0
   ext2spice rthresh 0
   ext2spice -d -o invx1_pex.spice
   ```
2. INVX2 :
   > Create Layout in Magic
   > Export SPICE netlist and 
   ``` cli
   extract all
   ext2spice lvs
   ext2spice -d -o invx2_layout.spice
   ```
   > Compare with Assignment_1 netlist
   ``` cli
   netgen -batch lvs "invx2_layout.spice INVX2" "invx2.ckt INVX2" /foss/pdks/sky130A/libs.tech/netgen/sky130A_steup.tcl
   ```
   > Export Parasitics
   ``` cli
   extract all
   ext2spice scale off
   ext2spice cthresh 0
   ext2spice rthresh 0
   ext2spice -d -o invx2_pex.spice
   ```
   

## Project 1

Project 1 implements the following cells:

| Cell | Directory | Available collateral |
| --- | --- | --- |
| AND2 (`A & !B`) | [`Project_1/AND`](Project_1/AND) | Pre-layout, layout, PEX, static-power, and analysis SPICE netlists; Magic layout; LEF; simulation results; Verilog and testbench. |
| Positive-edge DFF (`Q <= D`) | [`Project_1/DFF`](Project_1/DFF) | Pre-layout, layout, PEX, static-power, and analysis SPICE netlists; Magic layout; simulation results; Verilog and testbench. |
| Buffer experiments | [`Project_1/BUFF`](Project_1/BUFF) | Supporting/test circuit and layout files. |

The `*_analysis.ckt` files are intended to extract simulation parameters for
cell characterization. `*_spwr.ckt` files are for static-power analysis, and
`*_layout.spice` and `*_pex.spice` are the post-layout and
parasitic-extracted netlists, respectively.

## Tools

The examples below assume the relevant tools are installed and on `PATH`:

- `ngspice` for circuit simulation
- Magic for layout editing and extraction
- Netgen for LVS
- CharLib for Liberty (`.lib`) generation
- Icarus Verilog (`iverilog` and `vvp`) and GTKWave for digital verification

Most SPICE netlists reference the SKY130 model at
`/home/sumit/eda_tools/open_pdks/sky130/sky130A/libs.tech/ngspice/sky130.lib.spice`.
Update that path in a netlist or characterization configuration if the PDK is
installed elsewhere.

## Common workflows

Run these commands from the directory containing the files named in each
example.

### Simulate a SPICE netlist

```bash
cd Project_1/AND
ngspice and2.ckt
```

Use `and2_analysis.ckt` or `and2_spwr.ckt` for characterization and static
power runs. Equivalent DFF files are in [`Project_1/DFF`](Project_1/DFF).

### Open a layout and extract netlists

```bash
cd Project_1/AND
magic AND2.mag
```

Inside the Magic console, extract the layout, generate an LVS netlist, and
then generate a PEX netlist:

```tcl
extract all
ext2spice lvs
ext2spice -d -o and2_layout.spice
ext2spice scale off
ext2spice cthresh 0
ext2spice rthresh 0
ext2spice -d -o and2_pex.spice
```

To write LEF data from an open layout, run:

```tcl
lef write
```

### Run Verilog verification

```bash
cd Project_1/AND
iverilog -o and_sim and.v tb_and.v
vvp and_sim
gtkwave and_tb.vcd
```

For the DFF, substitute `Project_1/DFF`, `dff.v`, and `tb_dff.v`. The DFF
directory already includes [`dff.vcd`](Project_1/DFF/dff.vcd).



> Adjust the technology setup-file path if your SKY130 installation uses a different location.
