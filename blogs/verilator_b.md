# Verilator Beginner's Guide: Installation to Basic Usage and Features

By Shanzay Wasim

* * *

## Introduction

Verilator is a cycle-accurate, open-source Verilog and SystemVerilog simulator
that compiles HDL into C++ or SystemC code for high-speed simulation. It is
ideal for simulating synchronous digital logic circuits.

## Why Use Verilator?

  * **Speed:** Verilator's cycle-based simulation is significantly faster than most event-based simulators.
  * **Strict Code Compliance:** It promotes better coding practices by enforcing rules for synthesizable code.
  * **Cost:** It is free and open-source, making it accessible without the need for expensive licenses or hardware.

## Installation

To get started with Verilator, follow these steps:

### 1\. Install Verilator

    
    
    sudo apt-get install verilator

### 2\. Install GCC and Make

    
    
    sudo apt-get install build-essential

### 3\. Install GTKWave (optional, for viewing waveforms)

    
    
    sudo apt-get install gtkwave

Ensure you have a Linux environment for smooth installation and use. For other
platforms, visit the [Verilator Installation
Guide](https://verilator.org/guide/latest/install.html).

## Creating a Simple Project

Let's walk through creating a simple project to understand Verilator's
workflow. This project demonstrates how to simulate a simple counter module
using Verilator.

### Step 1: Create Your Verilog Module

Create a Verilog file named `counter.sv`:

    
    
    module counter #(parameter WIDTH = 4) (
        input clk,
        input rst,
        input enable,
        output reg [WIDTH-1:0] count
    );
        always @(posedge clk or posedge rst) begin
            if (rst) 
                count <= 0;
            else if (enable) 
                count <= count + 1;
        end
    endmodule

### Step 2: Convert Verilog to C++

    
    
    verilator --cc counter.sv

### Step 3: Write a Testbench

Create a C++ testbench file named `tb_counter.cpp`:

    
    
    #include <verilated.h>
    #include "Vcounter.h"
    
    int main(int argc, char** argv) {
        Vcounter* counter = new Vcounter;
        counter->clk = 0;
        counter->rst = 1;
        counter->enable = 0;
        
        // Reset the counter
        counter->eval();
        counter->rst = 0;
        
        // Enable and simulate some clock cycles
        for (int i = 0; i < 10; ++i) {
            counter->clk = !counter->clk;
            counter->enable = 1;
            counter->eval();
        }
        
        delete counter;
        return 0;
    }

### Step 4: Compile and Run

    
    
    verilator -Wall --trace -cc counter.sv --exe tb_counter.cpp
    make -C obj_dir -f Vcounter.mk Vcounter
    ./obj_dir/Vcounter

### Step 5: View Waveforms (Optional)

If you enabled tracing with the `--trace` option, view the generated waveform
using GTKWave:

    
    
    gtkwave obj_dir/waveform.vcd

## Notes

  * **Common Errors:** If you encounter errors during installation, ensure that all dependencies are correctly installed. Check the [Verilator FAQ](https://verilator.org/guide/latest/faq.html) for troubleshooting tips.
  * **Cross-Platform Use:** For Windows users, consider using WSL (Windows Subsystem for Linux) to set up a Linux-like environment.

## Output Expectations

When running the simulation, you should see the counter increment in the
terminal. If you've enabled waveform tracing, you can open `waveform.vcd` in
GTKWave to visualize the signal changes.

## Additional Features

### SystemVerilog Support

Verilator supports many SystemVerilog features, enhancing your ability to
create sophisticated testbenches and modules.

## Resources

  * [Verilator Documentation](https://verilator.org/guide/latest/)
  * [GTKWave Official Site](http://gtkwave.sourceforge.net/)
  * [Verilator GitHub Repository](https://github.com/verilator/verilator)

