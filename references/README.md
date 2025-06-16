
---

# 🛠 Vivado + Zybo Z7-20 Tcl Reference Guide

> This guide explains the necessary Tcl commands to automate a Vivado project for the **Zybo Z7-20**, targeting **SystemVerilog** designs. Each command is described with its purpose and example usage.

---

## 📁 Project Setup (One-Time)

These commands are used when creating a new project for the first time.

### 🎯 Create Project with Zybo Z7-20 Board

**Command**

```tcl
create_project <project_name> <project_dir> -board xilinx.com:zybo-z7-20:part0:1.1 -force
```

**Example**

```tcl
create_project zybo_project ./zybo_project -board xilinx.com:zybo-z7-20:part0:1.1 -force
```

> Uses Digilent's Zybo board preset to ensure correct part, I/O, and Zynq PS config.

---

### 🎯 Manually Set Part (Optional)

**Command**

```tcl
set_property PART xc7z020clg400-1 [current_project]
```

> Useful if board files aren't installed or you want a minimal project without the board preset.

---

### 🔎 Verify Board Assignment

**Command**

```tcl
current_board_part -verbose
```

**Output**
Shows board, part, and preset configuration info to confirm correct setup.

---

## 🔁 Reusable Build Steps (Run Every Build)

These commands are used regularly to synthesize, implement, and export your design.

### 📂 Open Project

**Command**

```tcl
open_project ./zybo_project/zybo_project.xpr
```

---

### 💬 Set HDL Language

**Command**

```tcl
set_property target_language SystemVerilog [current_project]
```

---

### 📌 Set Top-Level Module

**Command**

```tcl
set_property top <top_module_name> [current_fileset]
```

**Example**

```tcl
set_property top top [current_fileset]
```

---

### 📁 Add Sources

**Command**

```tcl
add_files <path_to_sv_files>
```

**Example**

```tcl
add_files ./src/top.sv
```

---

### 📁 Add Constraints

**Command**

```tcl
add_files -fileset constrs_1 <path_to_xdc>
```

**Example**

```tcl
add_files -fileset constrs_1 ./constraints/zybo_z7.xdc
```

---

## 🧹 Clean Previous Builds

**Command**

```tcl
reset_run synth_1
reset_run impl_1
```

> Clears previous synthesis and implementation results.

---

## 🔨 Build Flow

### 🔧 Synthesis

**Command**

```tcl
launch_runs synth_1
wait_on_run synth_1
```

---

### 🧱 Implementation

**Command**

```tcl
launch_runs impl_1
wait_on_run impl_1
```

---

### 🧵 Generate Bitstream

**Command**

```tcl
launch_runs impl_1 -to_step write_bitstream
wait_on_run impl_1
```

---

## 📦 Export for Software (Vitis / PetaLinux)

**Command**

```tcl
write_hw_platform -fixed -include_bit -force ./export/zybo_hw.xsa
```

> Generates `.xsa` with bitstream for Vitis or embedded Linux workflows.

---

## 🧩 (Optional) Block Design with Zynq PS

### 🔄 Open Block Design

**Command**

```tcl
open_bd_design ./zybo_project.srcs/sources_1/bd/system/system.bd
```

---

### ✅ Validate and Save

**Command**

```tcl
validate_bd_design
save_bd_design
```

---

## 🚀 Batch Mode Usage

Run the Tcl file (e.g., `build.tcl`) in batch mode:

```bash
vivado -mode batch -source ./scripts/build.tcl
```

---

## 📚 References

* [UG835: Vivado Tcl Command Reference Guide](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2022_2/ug835-vivado-tcl-commands.pdf)
* [UG894: Vivado Design Suite User Guide – Tcl Scripting](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2022_2/ug894-vivado-tcl-scripting.pdf)
* [Digilent Zybo Z7-20 Reference Manual](https://digilent.com/reference/programmable-logic/zybo-z7/reference-manual)

---

Let me know if you want this as a downloadable `.md` file or integrated into a GitHub-friendly project template.
