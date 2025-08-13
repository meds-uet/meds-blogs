# Makefile Tutorial for SystemVerilog Users

By Moazzam Ali

* * *

## What is a Makefile?

A Makefile is a script used by the `make` tool to automate repetitive tasks —
such as compiling SystemVerilog files, running simulations, generating
waveform files, and cleaning up your workspace.

Makefiles are especially useful in HDL design because simulation workflows
often involve many files and steps.

## Why Use Makefiles in SystemVerilog Projects?

  * ✅ Saves time: no need to retype commands
  * ✅ Only recompiles what changed
  * ✅ Keeps simulation workflow organized
  * ✅ Easy to extend for multiple testbenches, modules, or waveform options

## Basic Structure of a Makefile

    
    
    target: prerequisites
    [TAB] recipe (command)
    

### 1\. Target

The target is what you want to build or do — like compile, simulate, or clean.

Example:

    
    
    compile: rtl/top_module.sv tb/top_module_tb.sv

### 2\. Prerequisites (Dependencies)

These are files or other targets that must exist or be updated before the
current target runs.

Example:

    
    
    simulate: compile

### 3\. Recipe (Commands)

Shell commands executed to perform the target task. Must begin with a TAB.

    
    
    vlog rtl/top_module.sv tb/top_module_tb.sv

### 4\. Variables

Use variables to make your Makefile more readable:

    
    
    SRC = rtl/top.sv tb/top_tb.sv
    TOP = top_tb
    
    vlog $(SRC)
    vsim $(TOP)

### 5\. Special Built-in Targets

Target| Purpose  
---|---  
all| The default target when you run make  
clean| Removes simulation artifacts  
.PHONY| Declares targets that are not actual files  
  
Example:

    
    
    .PHONY: clean
    clean:
        rm -rf work transcript vsim.wlf
    

## A SystemVerilog Simulation Example

    
    
    # Variables
    TOP_MODULE = top_tb
    SRC_FILES = rtl/top.sv tb/top_tb.sv
    WORK_DIR = work
    
    # Default target
    all: compile simulate
    
    # Compile SystemVerilog files
    compile:
    	@echo "Creating work library..."
    	vlib $(WORK_DIR)
    	@echo "Compiling source files..."
    	vlog -work $(WORK_DIR) $(SRC_FILES)
    
    # Run the simulation in console mode
    simulate: compile
    	@echo "Running simulation..."
    	vsim -c -do "run -all; quit" -L $(WORK_DIR) $(TOP_MODULE)
    
    # Run the simulation in GUI mode
    wave: compile
    	@echo "Opening GUI simulation..."
    	vsim -do "run -all; quit" -L $(WORK_DIR) $(TOP_MODULE)
    
    # Clean up
    .PHONY: clean
    clean:
    	@echo "Cleaning up..."
    	rm -rf $(WORK_DIR) transcript vsim.wlf
    

## Key Concepts Recap

  * **Target:** A named action or file to build (compile, simulate, etc.)
  * **Prerequisite:** Files or tasks required before running a target
  * **Recipe:** Shell commands for performing a target
  * **Variable:** A named value like `SRC_FILES` to simplify code
  * **.PHONY:** Declares targets that are not actual files

