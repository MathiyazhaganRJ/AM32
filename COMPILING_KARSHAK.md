# AM32 Custom Firmware Compilation Guide (WSL)

This document outlines the steps required to compile a custom AM32 ESC firmware target (such as `REF_L431_KARSHAK`) on a Windows system using the Windows Subsystem for Linux (WSL).

## 1. Prerequisites (New System Setup)

If you are setting this up on a brand new Windows computer, you need to install the following tools first:

1. **Install Git:** Download from [git-scm.com/download/win](https://git-scm.com/download/win)
2. **Install WSL:** Open PowerShell as Administrator and run:
   ```powershell
   wsl --install
   ```
   *(Restart your computer if prompted).*
3. **Install the ARM Compiler:** Open your terminal, type `wsl` to enter Linux, and run:
   ```bash
   sudo apt update
   sudo apt install make gcc-arm-none-eabi python3
   ```

## 2. Defining a Custom Target

Firmware configurations are located in `Inc/targets.h`. To create a custom target, define a new `#ifdef` block. 
**Note:** The target name must be strictly UPPERCASE.

Example for `REF_L431_KARSHAK`:
```c
#ifdef  REF_L431_KARSHAK
#define FILE_NAME               "REF_L431_KARSHAK"
#define FIRMWARE_NAME           "L431 KARSHAK"
#define DEAD_TIME               80
#define HARDWARE_GROUP_L4_A
#define COMP_ORDER_L4_A_045
#define TARGET_VOLTAGE_DIVIDER  79   // Calibrated for user's hardware
#define USE_SERIAL_TELEMETRY
#define RAMP_SPEED_LOW_RPM 1
#define RAMP_SPEED_HIGH_RPM 1
#define MILLIVOLT_PER_AMP       50
#endif
```

## 3. Compiling the Firmware

Once your target is defined in `targets.h`, open PowerShell in the AM32 directory and run the build commands using WSL.

**Important:** The target name you type in the command line is **case-sensitive** and must exactly match the uppercase name in `targets.h`.

```powershell
# Optional: Clear old compiled files if you want to force a clean build
wsl make clean

# Compile the firmware for your specific target
wsl make REF_L431_KARSHAK
```

## 4. Retrieving the Firmware

When the compilation finishes successfully (Exit code: 0), your new firmware files will be generated in the `obj` directory:
- `obj/AM32_REF_L431_KARSHAK_2.20.bin`
- `obj/AM32_REF_L431_KARSHAK_2.20.hex`

You can then use the [AM32 Web Configurator](https://am32.ca/configurator) to flash these files directly to your ESC.
