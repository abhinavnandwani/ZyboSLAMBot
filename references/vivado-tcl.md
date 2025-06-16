######################################################################
# Vivado Build Script for Zybo Z7-20 SystemVerilog Project
# Author: [Your Name]
# Board: Zybo Z7-20 (xilinx.com:zybo-z7-20:part0:1.1)
# Description: Fully documented Tcl build script with Markdown-style
#              comments for learning and automation.
######################################################################

######################################################################
# 🔧 Section 1: One-Time Project Setup Commands
# Use these only once when creating a new project from scratch.
######################################################################

# ## Create a new project targeting the Zybo Z7-20 board
# This uses Digilent's board preset for correct part, pins, and PS setup.
# Example:
#   create_project my_proj ./my_proj -board xilinx.com:zybo-z7-20:part0:1.1 -force
# Note: Run once and remove/comment if project is already created.
#
# Uncomment to use:
# create_project zybo_project ../zybo_project -board xilinx.com:zybo-z7-20:part0:1.1 -force

# ## (Optional) Manually set board and part
# set_property BOARD_PART xilinx.com:zybo-z7-20:part0:1.1 [current_project]
# set_property PART xc7z020clg400-1 [current_project]

# ## Confirm board assignment
# current_board_part -verbose


######################################################################
# 🔁 Section 2: Reusable Build Commands
# These are used every time you build the project.
######################################################################

# ## Open the existing Vivado project
open_project ../zybo_project.xpr

# ## Ensure SystemVerilog is the target HDL
# Set only once per project unless changed
set_property target_language SystemVerilog [current_project]

# ## Define your top-level module
# Replace 'top' with the actual name of your top-level SystemVerilog module
set_property top top [current_fileset]

# ## Add source files (.sv)
# Use relative or absolute paths. Add multiple files if needed.
add_files ../src/top.sv

# ## Add constraint files (.xdc)
add_files -fileset constrs_1 ../constraints/zybo_z7.xdc


######################################################################
# 🧹 Section 3: Clean Build (Optional but Recommended)
######################################################################

# ## Reset previous synthesis and implementation runs
reset_run synth_1
reset_run impl_1


######################################################################
# 🏗 Section 4: Synthesis, Implementation, Bitstream
######################################################################

# ## Launch synthesis
launch_runs synth_1
wait_on_run synth_1

# ## Launch implementation
launch_runs impl_1
wait_on_run impl_1

# ## Generate bitstream
launch_runs impl_1 -to_step write_bitstream
wait_on_run impl_1


######################################################################
# 📦 Section 5: Export Hardware (for Vitis or PetaLinux)
######################################################################

# ## Export the .xsa hardware platform including the bitstream
# Required for software development or Linux boot integration
write_hw_platform -fixed -include_bit -force ../export/zybo_hardware.xsa


######################################################################
# 🧩 Section 6: Optional – Block Design Workflow
# If you're using a Zynq Processing System in a block design (.bd)
######################################################################

# ## Open and validate a block design
# Uncomment if needed
# open_bd_design ./zybo_project.srcs/sources_1/bd/system/system.bd
# validate_bd_design
# save_bd_design


######################################################################
# ✅ End of Script
# You can run this file in batch mode:
#   vivado -mode batch -source scripts/build.tcl
######################################################################
