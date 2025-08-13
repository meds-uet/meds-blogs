# Getting Started with Gem5-CVA6

By Shanzay Wasim

* * *

## Introduction to GEM5-CVA6

Gem5 is a powerful computer architecture simulator that researchers and
developers use to explore and analyze computer system performance. With its
highly modular and extensible design, Gem5 provides a flexible platform for
studying processor design, memory hierarchies, and system-level interactions.
It supports a wide range of systems, from simple uniprocessor machines to
complex multicore setups, making it invaluable for both academic research and
industry development.

## Setting up GEM5-CVA6

Now that you have gotten some basic understanding of what gem5 is, let’s dive
into the steps to set up GEM5-CVA6 in your system.

### Step 1: Cloning Repositories

Open your terminal and execute the following commands:

    
    
        git clone https://github.com/UmerShahidengr/gem5 gem5_cva6
        git clone https://github.com/darchr/riscv-validation.git
        

The _darchr/gem5_ repo was forked to keep track of changes made when modelling
cva6 on the gem5 simulator

### Step 2: Changing Directory

Navigate to the `gem5_cva6` directory:

    
    
        cd gem5_cva6
        

### Step 3: Switching Branch

Switch to the required branch:

    
    
        git checkout --track origin/CVA6/riscv-validation
        

### Step 4: Installing Libraries

Install the necessary libraries with:

    
    
        sudo apt-get install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python-dev python
        

If you encounter errors related to the Python library, resolve them by
installing the following:

![](./images/GEM5-CVA6_a_1.png)

To resolve this, install following libraries

    
    
        sudo apt-get install python3-dev
        sudo apt-get install python3
        sudo apt-get install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python3-dev
        

### Step 5: Compilation

Compile Gem5 for RISCV in the `gem5_cva6` directory using:

    
    
        scons build/RISCV/gem5.opt -j<threads></threads>
        

Replace `` with the number of cores in your PC plus one. To check the number
of cores, press `Ctrl + Shift + Esc` to open the Task Manager, select the
Performance tab, and note the number of cores. For example, if you have 4
cores, replace `<threads>` with 5:

    
    
        scons build/RISCV/gem5.opt –j5
        

If you receive the error “collect2: fatal error: ld terminated with signal 9
[killed]”, increase the swap space. Check the available swap space with:

![](./images/GEM5-CVA6_a_2.png)

    
    
        free –h
        

To increase swap space, create a `.wslconfig` file with:

    
    
        # Settings apply across all Linux distros running on WSL 2
        [wsl2]
    
        # Sets amount of swap storage space to 8GB, default is 25% of available RAM
        swap=20GB
    
        # Turn off default connection to bind WSL 2 localhost to Windows localhost
        localhostforwarding=true
        

Keep the filepath as:

    
    
        <Windows Drive>:\Users\<Username>\.wslconfig
        

Check the available swap space:

    
    
        free –h
        

After creating the file, restart your terminal and recheck the swap space. If
the space hasn’t increased, ensure the filename is exactly `.wslconfig`.

### Step 6: Checking Host

Determine whether you are working on a RISCV or non-RISCV host by running:

    
    
        uname -m
        

If the output is `x86_64`, you are on a non-RISCV host. If the output is
`riscv`, you are on a RISCV host. On a non-RISCV host, install the RISCV GCC
compiler with: **sudo apt install gcc-riscv64-linux-gnu**

### Step 7: Validation

Navigate to the `riscv-validation` repository:

    
    
        cd riscv-validation
        

**Note:** Ensure you are not in the `gem5_cva6` directory before this step.

### Step 8: Compiling Benchmarks

Compile the benchmarks for RISCV using:

    
    
        ./build_benchmarks.sh <C_compiler_name></C_compiler_name>
        

Replace <C_compiler_name> with:

  * `riscv64-linux-gnu-gcc` (for non-RISCV hosts)
  * `gcc` (for RISCV hosts)

### Step 9: Revisiting Repository

Return to the `gem5_cva6` directory:

    
    
        cd gem5_cva6
        

**Note:** Make sure you are out of the “riscv-validation” directory before
this step.

### Step 10: Running Benchmarks

Run benchmarks on gem5 with:

    
    
        ../riscv-validation/scripts/gem5_bench.sh configs/example/gem5_library/cva6-run.py
        ../riscv-validation/microbenchmark_suite-bins 100
        

If you are working on WSL (Windows Subsystem for Linux), you may you encounter
the “Cannot Allocate Memory” error like shown in figure below:

![](./images/GEM5-CVA6_a_3.jpeg)

To resolve this you need to increase the swap space as described in Step 5.

### Step 11: Finalization

Verify that Gem5 is set up correctly by checking the output:

    
    
        cat microbenchmark_suite-out/gem5_microbenchmark_suite.csv
        

The output should be as shown in the figure below:

![](./images/GEM5-CVA6_a_4.jpeg)

## TESTING GEM5

### Overview

    
    
        daqrcggem5_cva6
        vim configs/example/gem5_library/cva6-run.py
        ls src/python/gem5/prebuilt/cva6/
        

  1. Run and understand the Hello World file as follows: 
     * Go to the <gem5_source> directory (cd gem5_cva6)
     * Type: _vim configs/example/hmc_hello.py_ on the terminal

This will open the Gem 5 hmc_hello.py file.

  2. Run and understand the cva6-run.py file as follows: 
     * Type: _vim configs/example/gem5_library/cva6-run.py_ on the terminal
  3. Study the cva6 .py files: 
     * Go to the <gem5_source> directory (cd gem5_cva6)
     * Type: _ls src/python/gem5/prebuilt/cva6/ on the terminal_

To open a .py file type vim <path to .pyfile>

For Example:

    
    vim src/python/gem5/prebuilt/cva6/cva6_board.py
                

will open the cva6_board.py file.

  4. Study the CVA6 Core and Draw a block diagram of the Board (with its specs).

### Conclusion

You are now ready to work with Gem5 on your PC. If all the testing steps work
correctly, your setup is successfully installed. You can start simulating and
exploring computer architecture!

