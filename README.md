# Intel 12/13/14th gen undervolting guide

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Before you start](#before-you-start)
  - [Basics](#basics)

## Before you start
### Basics
TLDR: Why undervolt `UV`? -> lower temps, better performance, longer lifespan of your CPU

A CPU core needs a certain amount of voltage at a certain clock speed for it to work. For example, if you want a core to run at 5 GHz, it may need a minimum of 1.200 V to work. 
The higher the clock speed, the higher the required voltage, so 5.5 GHz may need at least 1.300 V. This required minimum voltage `[Vmin @ x GHz]` is individual for each CPU!

So how does your PC ensure this voltage is always available, so the cores don't crash because of too little voltage? Voltage regulation basics:
- Each CPU gets tested in the factory by intel for its quality of the cores. Then intel sets a table where for every GHz point there is a voltage specified. It's called the `[V/F table]`, voltage/frequency table, or the core `[VID table]`.
- This `[VID]` is the value for the voltage that get's requested by the CPU.
- The hardware part on your motherboard, that's generating the voltage is called the `[VRM]`, voltage regulator module. It takes the `[VID]` value and generates the actual voltage: the `[VCORE]`

Because of an additional stability safety margin added to the `[VID table]` by intel, when you are running the system stock (not manually tuned), the `[VID]` and therefore the `[VCORE]` is considerably higher than the `[Vmin]` needed to run the CPU stable. This is intentinal to ensure all CPUs intel sends out will work! It's a good thing for stability. (...in theory, considering the degradation issues of 13/14th gen intel...)

The goal of undervolting the CPU is to reduce the voltage `[VCORE]` so it's as close as possible to the minimum required voltage `[Vmin]` while still being stable under all conditions. 

You want to do this because of multiple reasons:
- This simple formular `[W = V * A]` (Watt = Volt * Amper; Watt = power consumption) is the physics reason behind it
- `[A]` is dependet on the workload of the CPU. Heavy workloads need higher `[A]`
- Therefore, if you reduce the `[V]`, you reduce the `[W]` - the power consumption
- Because heat output is proportinal to the `[W]`, reducing the `[W]` will reduce the temperature of your CPU
- If you are hitting a power limit or temp limit, your CPU cores will clock down, to make sure they stay within those parameters. Reducing the `[V]` in this scenario will therefore increase the clock speeds and thus performance, because performance within the same generation is directly proportional to the clock speed.

- The silicon of a CPU naturally degrades over time while using it. "Degrade" basically means, the `[Vmin @ x GHz]` will increase over time (that's another reason why chip manufacturers have this pre-programmed safety margin). The higher the voltage, the faster the degredation process! Therefore lowering the delivered voltage `[VCORE]` will increase the lifetime of your CPU. Though this point is usually not important, because usually CPUs lasted for well over 10 years - even with stock settings! It's only important for the Intel Raptor-Lake CPUs (13/14th gen) because of the degradation issues.
  - I will not be going into any detail on the intel degredation problem. The only thing you need to know is: older BIOS versions include microcode that makes the CPU request dangerously high voltages that damage the CPU over time. According to Intel, updating the BIOS to the newest version should be enough to fix this. Many overclockers recommend to take additional measures by manually undervolting your CPU!

### Required Software
All the settings you need to undervolt will be in the BIOS of your motherboard. If you never changed a settings there, don't worry - as long as you work thoroughly and don't start randomly settings different stuff without research, nothing can happen. But be carefull, you can damage your CPU and system if you set a wrong value!

Appart from being able to enter the BIOS, you will need some software in Windows to confirm the stability and success of your settings:
- [HardwareInfo](https://www.hwinfo.com/download/)
- [YCruncher](https://www.numberworld.org/y-cruncher)
- [Bechmate](https://benchmate.org/)

There are other tools you can use to stablity test as well:
- [OCCT](https://www.ocbase.com/download)
- [CoreCycler](https://corecycler.com/)

### General Process
First of all, update your BIOS to the newest version. Every motherboard brand hast a slighly different way of doing that, so please research this yourself. Usually you do it by going to the website of your motherboard model, navigating to the "support" page, downloading the BIOS file onto a USB drive, booting into the BIOS and selecting a "update BIOS tool".

Make sure to read everything carefully. Don't just click through things in a rush, take your time on each step if you are doing it the first time!
