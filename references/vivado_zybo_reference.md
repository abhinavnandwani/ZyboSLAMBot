
# Vivado Tcl Reference Guide for Zybo Z7-20 (SystemVerilog)

## 1. Project Setup (One-Time)

These commands are used once when creating a new project targeting the Zybo Z7-20 board.

### Create New Project Using Board Preset

```tcl
create_project zybo_project ./zybo_project -board xilinx.com:zybo-z7-20:part0:1.1 -force
```

This creates a new Vivado project with the Zybo Z7-20 board configuration. It automatically sets the correct FPGA part and enables board-specific presets.

### Manually Set Part (Alternative to Board Preset)

```tcl
set_property PART xc7z020clg400-1 [current_project]
```

Use this if you prefer to manually configure the FPGA part without a board preset.

### Set Board Part Property

```tcl
set_property BOARD_PART xilinx.com:zybo-z7-20:part0:1.1 [current_project]
```

Used when reassigning a board after project creation.

### Verify Current Board

```tcl
current_board_part -verbose
```

Outputs the current board assignment and its associated part.

---

## 2. Project Opening and Setup (Every Time)

These commands are needed each time the project is opened and rebuilt.

### Open Existing Project

```tcl
open_project ./zybo_project/zybo_project.xpr
```

### Set Project Language to SystemVerilog

```tcl
set_property target_language SystemVerilog [current_project]
```

### Set Top-Level Module

```tcl
set_property top top [current_fileset]
```

Replace `top` with your actual top module name.

---

## 3. Add Design Files

### Add Source Files (SystemVerilog)

```tcl
add_files ./src/top.sv
```

You can add multiple files or directories.

### Add Constraints (XDC File)

```tcl
add_files -fileset constrs_1 ./constraints/zybo_z7.xdc
```

---

## 4. Clean Previous Build Artifacts

### Reset Synthesis and Implementation Runs

```tcl
reset_run synth_1
reset_run impl_1
```

Use this to ensure a clean build.

---

## 5. Run Design Flow

### Run Synthesis

```tcl
launch_runs synth_1
wait_on_run synth_1
```

### Run Implementation

```tcl
launch_runs impl_1
wait_on_run impl_1
```

### Generate Bitstream

```tcl
launch_runs impl_1 -to_step write_bitstream
wait_on_run impl_1
```

---

## 6. Export Hardware for Vitis or PetaLinux

### Export .xsa with Bitstream

```tcl
write_hw_platform -fixed -include_bit -force ./export/zybo_hw.xsa
```

Generates a hardware description file used for software development.

---

## 7. Optional: Block Design Flow

### Open Existing Block Design

```tcl
open_bd_design ./zybo_project.srcs/sources_1/bd/system/system.bd
```

### Validate and Save Block Design

```tcl
validate_bd_design
save_bd_design
```

---

## 8. Batch Execution

To run the full build script in batch mode:

```sh
vivado -mode batch -source ./scripts/build.tcl
```

---

## 9. References

- Vivado Tcl Command Reference (UG835): https://www.xilinx.com/support/documentation/sw_manuals/xilinx2022_2/ug835-vivado-tcl-commands.pdf
- Vivado Tcl Scripting User Guide (UG894): https://www.xilinx.com/support/documentation/sw_manuals/xilinx2022_2/ug894-vivado-tcl-scripting.pdf
- Digilent Zybo Z7-20 Reference Manual: https://digilent.com/reference/programmable-logic/zybo-z7/reference-manual
