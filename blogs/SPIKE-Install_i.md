# Getting Started with Spike: Installation and Usage

## An Introduction to SPIKE: The RISC-V Instruction Set Simulator

With the rising adoption of the RISC-V architecture, developers and hardware
engineers increasingly require dependable tools to simulate, test, and
validate RISC-V designs—especially in the absence of physical hardware. One
such essential tool in the RISC-V ecosystem is Spike, the official RISC-V
Instruction Set Simulator (ISS).

### **What is SPIKE?**

Spike is an open-source Instruction Set Simulator (ISS) that models the
behavior of RISC-V processors at the instruction level. Developed by the
RISC-V Foundation, Spike acts as the golden model—or reference
implementation—of the RISC-V architecture. It accurately simulates how a
RISC-V core executes instructions, manipulates registers, and interacts with
memory, enabling developers to verify the correctness of both hardware designs
and software running on RISC-V platforms.

While Spike’s primary purpose is to simulate RISC-V processors, it supports a
variety of configurations, including:

  * **RV32I** – the 32-bit base integer instruction set
  * **RV64I** – the 64-bit base integer instruction set
  * Optional extensions such as **M** (integer multiplication and division), **A** (atomic operations), **F** (single-precision floating point), **D** (double-precision floating point), and others

Spike functions as a virtual RISC-V processor, executing instructions and
enabling early-stage development, testing, and debugging—even in the absence
of actual hardware. This makes it an essential tool for researchers,
developers, and hardware designers working on RISC-V-based systems.

## **Main Functionalities of SPIKE**

**1\. Instruction Set Simulation**

At its core, Spike is built to simulate the RISC-V instruction set in both
32-bit and 64-bit configurations. It offers a straightforward yet accurate
emulation of how a RISC-V processor executes instructions, making it an ideal
tool for gaining insight into the interaction between instructions, registers,
and memory. Whether you're debugging low-level code or learning how RISC-V
works internally, Spike provides a reliable and transparent reference model.

**2\. Debugging and Verification**

Spike allows developers to closely observe the internal state of the processor
during program execution. It provides access to registers, memory locations,
and other key components of the processor’s state, making it especially useful
for debugging both assembly code and high-level code compiled for RISC-V. As
the official reference model, Spike serves as a reliable standard for
comparing the behavior of real hardware against a trusted, architecture-
compliant simulation.

**3\. Toolchain Integration**

Spike integrates seamlessly with the RISC-V toolchain, including popular
compilers like GCC and LLVM. Developers can compile RISC-V applications and
run them directly within Spike to test functionality, performance, and
correctness all without requiring physical hardware. This makes Spike a
powerful tool for rapid prototyping and early-stage software development.

**4\. Support for Custom Configurations**

Spike is highly configurable, allowing users to simulate various RISC-V
implementations and optional extensions. For instance, you can emulate a
processor with or without the M extension (for integer multiplication and
division) or the F extension (for floating-point operations). This flexibility
makes Spike an essential tool for diverse RISC-V projects, ranging from
resource-constrained embedded systems to complex high-performance computing
applications.

**5\. Reference Model**

In addition to serving as a development tool, Spike functions as a golden
reference model for validating custom hardware implementations of the RISC-V
architecture. Hardware engineers often rely on Spike to compare the behavior
of their designs against the expected output of a correctly functioning RISC-V
processor. This comparison helps identify discrepancies early in the design
process and ensures that the hardware conforms to the architectural
specification.

## **Installing Spike: A Step-by-Step Guide**

Installing Spike is relatively straightforward, especially on Unix-like
systems. The process involves setting up a few essential dependencies,
configuring the environment, and building the tool from source. In this blog,
we’ll guide you step-by-step through the installation process, explaining each
stage to help you understand not just how, but also why each step is
necessary.

**1\. Updating System**

This updates the list of available packages and their versions to ensure your
system is aware of the latest versions.

    
    
    sudo apt-get update
    

**2\. Installing Dependencies**

This command installs the essential dependencies for building Spike.
**Autoconf** and **automake** generate build configurations, while **gcc** and
**g++** are the compilers needed for code compilation. **Git** helps clone the
repository, and **libtool** simplifies handling shared libraries. Lastly,
**zlib1g-dev** provides the compression library required for building
components that need compression support.

    
    
    sudo apt-get install autoconf automake gcc git g++ libtool zlib1g-dev
    

**3\. Installing Device Tree Compiler**

This command installs the **Device Tree Compiler** , which is necessary for
building the Spike simulator. The Device Tree Compiler is used to compile
source code into binary representations for hardware configuration.

    
    
    sudo apt-get install device-tree-compiler
    

**4\. Cloning the Spike Repository**

This command clones the **Spike RISC-V ISA Simulator** repository from GitHub.
It fetches the latest source code needed to compile and install the Spike
simulator.

    
    
    git clone https://github.com/riscv-software-src/riscv-isa-sim.git
    

This command will clone the latest version of Spike (currently **v1.1.1**).
However, if you want to build an older version, navigate to the cloned
repository and run:

    
    
    git checkout v1.<version_tag>
    

For example, you can switch to one of the older available tags:

  * v1.0.0
  * v1.1.0

Replace ****with the specific version you want to install.

**5\. Building Spike**

This navigates into the cloned Spike repository.

    
    
    cd riscv-isa-sim
    

This creates a new directory called **build** where the Spike simulator will
be compiled.

    
    
    mkdir build
    

Moves into the newly created build directory.

    
    
    cd build
    

**6\. Configuring and Compiling Spike**

This runs the configuration script to prepare for the build process. The
--prefix=/opt/riscv argument specifies that the Spike simulator should be
installed in the /opt/riscv directory.

    
    
    ../configure --prefix=/opt/riscv
    

The make command compiles the source code. The -j$(nproc) flag utilizes all
available CPU cores to speed up the build process.

    
    
    make -j$(nproc)
    

This installs the compiled Spike binaries into the /opt/riscv directory.

    
    
    sudo make install
    

**7\. Setting Up Environment Variables**

Navigates to your home directory.

    
    
    cd ~
    

Opens the .bashrc file in the **nano** text editor. The .bashrc file is a
script that runs whenever you open a new terminal session. You’ll modify this
file to include the newly installed Spike binaries in your system’s PATH.

    
    
    nano ~/.bashrc
    

**8\. Adding Spike to PATH**

This command appends the directory /opt/riscv/bin to the system’s $PATH
environment variable, ensuring that the Spike commands can be executed from
any directory in the terminal.

    
    
    export PATH=/opt/riscv/bin:$PATH
    

After editing the .bashrc file, this command reloads it to apply the changes
immediately.

    
    
    source ~/.bashrc
    

**9\. Testing Spike**

This command checks whether Spike is installed correctly by displaying a help
message. If Spike is correctly installed, this command will print a list of
available options and usage information.

    
    
    spike --help
    

Verify whether the Spike proxy kernel (pk) is installed correctly, run the
following command:

    
    
    which pk
    

If it returns a valid path (e.g., /opt/riscv/bin/pk), it means pk is installed
and accessible via your environment.

If not, follow these steps to install it:

## **Installing SPIKE Proxy-Kernel**

**1\. Cloning the Proxy-Kernel Repository:**

This command clones the **RISC-V Proxy-Kernel** repository from GitHub. It
fetches the latest source code needed to compile and install the Proxy-Kernel.

    
    
    git clone https://github.com/riscv-software-src/riscv-pk.git
    

**2\. Building Proxy-Kernel**

This navigates into the cloned Spike repository.

    
    
    cd riscv-pk
    

This creates a new directory called **build** where the Spike simulator will
be compiled.

    
    
    mkdir build
    

Moves into the newly created build directory.

    
    
    cd build
    

**3\. Configuring and Compiling Spike**

This runs the configuration script to prepare for the build process. The
--prefix=/opt/riscv argument specifies that the Spike simulator should be
installed in the /opt/riscv directory and --host=riscv64-unknown-elf specifies
that our system if of 64-Bit.

    
    
    ../configure --prefix=/opt/riscv --host=riscv64-unknown-elf
    

The make command compiles the source code. The -j$(nproc) flag utilizes all
available CPU cores to speed up the build process.

    
    
    make -j$(nproc)
    

This installs the compiled Spike binaries into the /opt/riscv directory.

    
    
    sudo make install
    

**4\. Adding Proxy-Kernel to PATH**

This command appends the directory /opt/riscv/bin to the system’s $PATH
environment variable, ensuring that the pk commands can be executed from any
directory in the terminal.

    
    
    export PATH=/opt/riscv/bin:$PATH
    

After editing the .bashrc file, this command reloads it to apply the changes
immediately.

    
    
    source ~/.bashrc
    

## **Installing the RISC-V Toolchain**

After setting up Spike, you will need the RISC-V Toolchain to compile programs
for RISC-V targets.

**1\. Installing the RISC-V Toolchain**

This command installs the RISC-V **GCC cross-compiler** toolchain. It is used
to compile C and C++ code for RISC-V targets. The gcc-riscv64-unknown-elf
package allows compiling programs to run on the RISC-V ISA.

    
    
    sudo apt-get install gcc-riscv64-unknown-elf
    

If this doesn’t work, you can manually install the latest version of the
RISC-V toolchain compatible with your Ubuntu version from the official GitHub
releases page: 👉 https://github.com/riscv-collab/riscv-gnu-toolchain/releases

After downloading, extract the archive and add the bin directory of the
extracted toolchain to your ~/.bashrc file. For example:

    
    
    export PATH=/path/to/your/toolchain/bin:$PATH
    

**2\. Verifying the Installation**

This command checks if the RISC-V GCC toolchain was installed correctly by
printing the version of the RISC-V GCC compiler. If the installation was
successful, the toolchain version will be displayed in the terminal.

    
    
    riscv64-unknown-elf-gcc --version
    

## **SPIKE Usage and Commands**

SPIKE is a powerful tool for simulating RISC-V binaries. After successful
installation and setup, you can start using SPIKE to run your compiled RISC-V
applications.

The basic syntax to run a RISC-V binary with SPIKE is:

    
    
    spike pk <binary_file>
    

**Example:**

  * Sample C Program (demo.c)

int main() { int a = 5, b = 3; int c = a + b; return c; }

  * Compile this C code with RISC-V GCC toolchain:

riscv64-unknown-elf-gcc -static -o demo demo.c

  * View Disassembly of Binary:

riscv64-unknown-elf-objdump -D demo

  * Now run it on SPIKE:

spike pk demo

It doesn't show any output on the terminal as our code is not printing
anything. However, we can see the value stored in the last register by using:

    
    
      echo $?
    

**Additional Options we can use on spike**

  1. Simulate only a specific ISA (e.g., RV32I):

spike --isa=RV32I pk demo

  2. Enable logging:
         
         spike -l pk demo
         

This shows a log of instructions being executed.

  1. Specify number of cores:
         
         spike -p2 pk demo
         

Simulates 2 processor cores.

  1. Run SPIKE with memory size specification:
         
         spike -m1024 pk demo
         

Allocates 1024 MB of RAM to the simulation.

**Debugging RISC-V Programs in SPIKE**

SPIKE is not a traditional debugger, it provides debugging support through:

  * Instruction-level logging (-l)
  * Register dumps
  * Integration with GDB via RSP

## **Conclusion**

By following the steps outlined above, you should now have a working
installation of the Spike RISC-V ISA simulator, along with a solid
understanding of how to compile, run, and debug RISC-V programs using it.
Whether you're developing embedded applications, testing custom RISC-V
extensions, or verifying hardware behavior, Spike provides a robust and
flexible environment to support your work. With its integration into the
RISC-V toolchain and support for debugging and customization, Spike is an
essential tool for any developer or researcher working in the RISC-V
ecosystem.

