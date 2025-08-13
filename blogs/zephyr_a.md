# Running Zephyr on UETRV-PCore with RISC-V

By Muhammad Ramzan

* * *

**What is Zephyr?**  
Zephyr is a scalable, open-source, real-time operating system (RTOS) designed
for resource-constrained devices. It supports a wide range of hardware
architectures, including RISC-V, ARM, x86, and others, making it an ideal
choice for embedded systems and Internet of Things (IoT) applications. Zephyr
provides a lightweight kernel, a configurable set of features, and a rich
ecosystem of libraries and tools, enabling developers to build efficient and
reliable applications.

**What is UETRV-PCore?**  
UETRV-PCore is a 32-bit RISC-V System on Chip (SoC) developed at UET Lahore.
It is built around a 5-stage pipelined processor that supports the
RV32IMACZicsr instruction set architecture (ISA). The design includes support
for multiple privilege modes (Machine, Supervisor, and User), making it
suitable for both embedded and general-purpose computing applications. The SoC
features an MMU based on the Sv32 standard, along with 32KB instruction and
data caches to optimize performance. Additionally, it integrates essential
peripherals such as UART, SPI, and PLIC, enabling communication and interrupt
handling. Capable of running Linux, UETRV-PCore also includes comprehensive
architecture compatibility tests using RISOF, ensuring robustness and
compliance with the RISC-V specifications.

![](./images/zephyr_a_1.png)

Fig 1.1 : System Design Overview

### Key Features:

  * **Core:** RV32IMACZicsr, 5-stage pipeline, supports privilege modes.
  * **Caches:** 32KB instruction cache, 32KB data cache.
  * **MMU:** Sv32-based, shared page-table walker.
  * **Peripherals:** UART, SPI, PLIC, CLINT.
  * **Performance:** 2.0 Coremark/MHz.
  * **Configurable:** Cache sizes, TLB entries.

### Simulation and Linux Boot:

The design supports simulation using Verilator and booting Linux via prebuilt
images. Verilator models allow custom parameters (imem, max_cycles, vcd) and
waveform generation for debugging. Linux boot images integrate OpenSBI and
BusyBox-based rootfs.

### FPGA Implementation:

Supports Nexys A7 (100T) with Vivado. Prebuilt bitstreams and Linux boot
images are available.

### Getting Started:

Install required tools (gcc-riscv64-unknown-elf, Verilator) and use provided
Makefile to build, simulate, and test.

For detailed documentation, visit [UETRV-PCore Documentation.](https://uetrv-
pcore-doc.readthedocs.io/en/main/)

### Exploring Zephyr on UETRV-PCore

In our journey to run Zephyr on the UETRV-PCore, we navigated several
challenges, from setting up the RISC-V toolchain to configuring the device
tree and adapting Zephyr to work seamlessly with the SoC. This blog documents
our step-by-step approach, covering everything from installing the necessary
tools and dependencies to debugging and optimizing Zephyr for the UETRV-PCore.

Our exploration highlights key aspects of integrating Zephyr with a custom
RISC-V processor, offering insights into the process and overcoming common
hurdles, ultimately achieving a functional and efficient implementation.

### Prerequisites:

Before we began the installation process, we ensured that we had the following
tools and environments set up:

  1. riscv32-elf-ubuntu-22.04-gcc-nightly-2024.04.12-nightly
  2. Spike
  3. Qemu
  4. ModelSim
  5. GTKWave

### RISC-V Toolchain and Verilator

We needed to install the RISC-V toolchain to compile Zephyr applications and
Verilator to simulate the UETRV-PCore processor. The following command helped
us install the necessary tools:

    
    
    sudo apt-get install -y gcc-riscv64-unknown-elf verilator gtkwave

### Zephyr SDK and Environment Setup

The Zephyr SDK was essential for building Zephyr applications. We followed the
official Zephyr Project documentation to download and set up the SDK. Once
downloaded, we activated the environment with the following commands:

    
    
    source ~/zephyrproject/.venv/bin/activate
    export ZEPHYR_TOOLCHAIN_VARIANT=zephyr
    export ZEPHYR_SDK_INSTALL_DIR=~/zephyr-sdk-0.16.8/

### Building Zephyr for UETRV-PCore

#### Updating the Zephyr Device Tree (DTS)

Zephyr uses Device Tree files (.dts) to configure hardware. Since the UETRV-
PCore employed a different UART driver (SiFive UART), we needed to make
modifications. Specifically, we updated the virt-riscv.dtsi file according to
the sifive_uart driver to match the hardware architecture of UETRV-PCore.

#### Building Zephyr for QEMU-RISC-V

Before using physical hardware, we decided to test Zephyr with a RISC-V
simulation on QEMU. We built the hello_world application for the qemu_riscv32
board:

    
    
    cd ~/zephyrproject/zephyr/samples/hello_world west build -b qemu_riscv32 --pristine
    west build -t run

#### Testing Zephyr on QEMU

Once the build was complete, we used QEMU to simulate the RISC-V environment.
We ran the application and verified that "Hello, World!" was printed on the
console. This confirmed that the Zephyr configuration and application were
working correctly:

    
    
    west build -b qemu_riscv32 --pristine 
    west build -t run

#### Building Zephyr.elf for UETRV-PCore

  * **Converting .elf to imem.txt:**

To run Zephyr on UETRV-PCore, we needed to convert the zephyr.elf file into a
format compatible with UETRV-PCore's custom memory map. We followed these
steps to generate the imem.txt file:

  * **Disassemble the zephyr.elf:**

We generated a disassembly listing for zephyr.elf:

    
    riscv64-unknown-elf-objdump -d zephyr.elf > zephyr.dump

  * **Convert zephyr .elf to Binary:**

We created a binary file from the zephyr.elf:

    
    riscv64-unknown-elf-objcopy -O binary zephyr.elf zephyr.bin

  * **Generate Hexadecimal Representation:**

We used the xxd tool to generate a hexadecimal representation:

    
    xxd -p -c 8 zephyr.bin > imem.txt

  * **Zipping the imem.txt File:**

To facilitate transferring the binary file to the UETRV-PCore, we zipped the
imem.txt file:

    
    zip imem.zip imem.txt

#### Using Spike and ModelSim for Verification

In addition to QEMU, we used **Spike** , a RISC-V ISA simulator, to validate
the behavior of Zephyr running on the UETRV-PCore. Spike allowed us to
simulate the execution of Zephyr on a more accurate model of the RISC-V
architecture.

We also used **ModelSim** for hardware-level verification and simulation of
the UETRV-PCore. ModelSim enabled us to simulate the RTL design of the
processor and verify its interactions with Zephyr.

#### Zephyr RTOS - Device Tree Handling:

#### Static Configuration at Build Time

  * **Processes DTS During Build:** Zephyr processes Device Tree Source (DTS) files directly during the build phase.
  * **Generates Static C Headers:** From the DTS, static C headers are generated to configure the hardware at compile-time.
  * **Requires Firmware Rebuild for Changes:** Any modifications in hardware configuration require a full firmware rebuild to take effect.
  * **Optimized for Embedded Systems:** This static approach is beneficial for resource-constrained systems, minimizing runtime overhead and optimizing for deterministic performance.

![](./images/zephyr_a_2.png)

Fig 1.2 : Device Tree Handling

This summary focuses on the key aspects of how Zephyr RTOS handles Device Tree
in a static and compile-time manner, making it well-suited for embedded
systems.

#### Running Zephyr on Physical UETRV-PCore

When we were ready to run Zephyr on actual hardware, we followed these steps
to load the binary image into the UETRV-PCore and begin the simulation
process:

**Replacing the Generated IMEM File:**

We replaced the previously generated imem.txt file with the new zephyr.elf
file in the /UETRV-PCore/sdk/example-linux directory.

#### Simulating Linux Boot:

To simulate the Linux booting process using the pre-built image, we executed
the following command:

    
    
    make sim-verilate-linux

However, when we checked the UART output in the uart_logdata.log file, we
observed that the log contained garbage data instead of the expected output.

### Debugging and Resolving Issues

After encountering the issue of garbage data in the UART log, we analyzed the
potential reasons for the mismatch and identified the following causes:

  1. **Incorrect UART Driver Configuration:**

The garbage data likely occurred because the ns16550 UART driver was used in
the original configuration. However, UETRV-PCore uses the sifive_uart driver,
which has different register addresses and functionality. This mismatch caused
the incorrect behavior, as the ns16550 driver was not compatible with the
UETRV-PCore's hardware configuration.

**Solution:** We modified the virt-riscv.dtsi file to replace the ns16550
driver with the sifive_uart driver, which is the correct driver for the UETRV-
PCore. This ensured that the UART interface was properly configured and
aligned with the hardware.

  2. **Check Register Mapping:**

Upon closer inspection, we realized that there were differences in the
register addresses between the sifive_uart driver and the ns16550 driver.
Specifically, the registers used by the sifive_uart in UETRV-PCore were not
available in the ns16550 driver, and some of the register addresses were
different, which caused the garbage data issue.

**Solution:** We reviewed the memory-mapped registers for both the Zephyr
application and the UETRV-PCore. By aligning the registers in the **device
tree** (specifically in the virt-riscv.dtsi file), we ensured that the UART's
memory-mapped I/O addresses matched the configuration of the sifive_uart
peripheral in UETRV-PCore.

  3. **Using Debugging Tools:**

To further troubleshoot and verify the correctness of our simulation, we used
debugging tools such as **gtkwave** and **Verilator**. These tools helped us
analyze the simulation waveform, inspect the timing, and check if data was
being correctly transmitted over the UART interface.

**Solution:** By running simulations with **Verilator** and inspecting the
waveforms in **gtkwave** , we were able to verify the correct functionality of
the UART interface and ensure that the Zephyr application was executing
properly on the UETRV-PCore.

### Conclusion

Running Zephyr on UETRV-PCore provided us with a powerful and efficient
platform for embedded applications using the RISC-V architecture. By carefully
addressing the challenges related to UART driver mismatches and ensuring
proper register mapping, we were able to successfully run Zephyr applications
on both simulation environments and real hardware.

With further optimizations, Zephyr on UETRV-PCore offers a solid foundation
for developing embedded systems, especially in educational settings where
understanding processor design and real-time operating systems is essential.

### Useful Links

  * **Zephyr Project Documentation:** [Zephyr Project Documentation](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)
  * **Zephyr GitHub Repository:** [Zephyr GitHub](https://github.com/zephyrproject-rtos/zephyr)
  * **UETRV-PCore GitHub Repository:** [UETRV-PCore GitHub](https://github.com/ee-uet/UETRV-PCore.git)
  * **Zephyr SDK:** [Zephyr SDK Installation Guide](https://docs.zephyrproject.org/latest/develop/toolchains/zephyr_sdk.html#toolchain-zephyr-sdk)

