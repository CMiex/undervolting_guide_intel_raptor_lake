# Intel 12/13/14th gen undervolting guide

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Before you start](#before-you-start)
  - [Basics](#basics)
  - [Required Software](#required-software)
- [General Process](#general-process)
  - [Baseline](#baseline)
  - [Loadline Calibration tuning](#loadline-calibration-tuning)
  - [Negative VID offset](#negative-vid-offset)
  - [ASUS](#asus)
  - [MSI](#msi)
  - [GIGABYTE](#gigabyte)

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
- [HardwareInfo](https://www.hwinfo.com/download/) *HwInfo*
- [YCruncher](https://www.numberworld.org/y-cruncher)
- [BechMate](https://benchmate.org/)

There are other tools you can use to stablity test as well:
- [OCCT](https://www.ocbase.com/download)
- [CoreCycler](https://corecycler.com/)

## General process
**Make sure to read everything carefully. Don't just click through things in a rush, take your time on each step and in the BIOS if you are doing this for the first time!**

First of all, update your BIOS to the newest version. Every motherboard brand has a slighly different way of doing this, so please research this yourself. Usually you do it by going to the website of your motherboard model, navigate to the "support" page, download the BIOS file onto a USB drive, boot into the BIOS and select a "update BIOS tool" to read the BIOS file on the drive and install the update.

### Baseline
Before changing any settings in the BIOS, let's get a baseline for the performance when everything is stock. For that, start `HwInfo`:
- at the start you can select "Sensors-only", or when you open the "Full mode", click on "Sensors"

  <img width="879" height="360" alt="HwInfo_Full_edit" src="https://github.com/user-attachments/assets/602a4a82-2bbe-4c53-8771-2e7767122a2a" />

- in `HwInfo` we mainly want to look at the following values: temperature, power consumption and voltage. Click on the little arrow beside "Core Clocks" to show all cores.

  <img width="697" height="1384" alt="HwInfo_Sensors" src="https://github.com/user-attachments/assets/1b8e46da-72d6-4c95-a067-2f5e2e5c9631" />

- to get a better overfew of what's happening, double click the values to open graphs like that:

  <img width="570" height="535" alt="image" src="https://github.com/user-attachments/assets/b1037de0-e3f8-460a-a265-9b8c145f5713" />

- click "Auto fit" so you can quickly read the min and max values

Next step: Start `BenchMate` and start Cinebench (R23 or 2024 are most commonly used to compare)
- start a benchmark run and note down the score you get, as well as the temperature, power consumption and voltage. Do this 3 times for the "Multi Core" test and once for the "Single Core" test
- If your Cinebench doesn't show the option to only run a single cycle, you can click on "File" -> "Advanced Options", then set "Minimum Test Duration" -> "Off"

  <img width="788" height="522" alt="Cinebench" src="https://github.com/user-attachments/assets/e3355839-042d-4dbd-9397-21328e117ccc" />

- While running the tests, check if you are running into any temperature or power limits. You can see them here:

  <img width="676" height="1236" alt="HwInfo_Performance_Limit" src="https://github.com/user-attachments/assets/43264615-5cbe-4a8c-94f4-6462e2d6dbb7" />

- Note this down as well, or take a screenshot of it

### Loadline Calibration tuning
*If you get confused by this, you can skipt this part and go directly to [the next part](#asus). It's not essential.*

There are different approaches to undervolting. The one that results in the lowest average `[VCORE]` is by tuning the `Loadline Calibration` for a small `vdroop` and settings a `negative VID offset`. 
`vdroop` is the voltage drop that occures when the CPU is under load. 
So if you have a voltage of 1.450 V while you are running a Single Core workload and a voltage of 1.300 V with a Multi Core workload, while the clocks are the same, your `vdroop` would be 0.150 V, or 150 mV. 
By setting a stronger loadline you lower the voltage difference between light load (single core) and heavy load (multi core). 

The goal is to get a small droop of anywhere between 20 and 80 mV (0.02 and 0.08). So the single core workload `[VCORE]` should be optimally max. 0.08 V higher than the multi core workload `[VCORE]`, while in both scenarios the cores are clocked the same. 
If your core clocks are not the same because you hit a power limit or thermal limit, or because the single core boost clock is higher than the multi core boost clock, you can set your `P-Core Ratio` to be lower (f.a. to 50 or lower) for all cores and test the `vdroop` that way. 

How to set the `Loadline Calibration` and the `Core Ratio` is explained in the motherboard-brand specific settings section of this guide, [the next part](#asus).

Generally: the more cores, the larger the vdroop, so a *stronger* `Loadline Calibration` is needed. The process of setting the `Loadline Calibration` is best done while setting and testing for the optimal `negative VID offset`. The *stronger* the `Loadline Calibration`, the smaller the `vdroop`, therefore the voltage stays higher, therefore a larger `negative VID offset` is possible. 

### Negative VID offset
What is a `negative VID offset`? If we think back to the [basics](#basics), the `[VID]` is the voltage that the CPU requests. So by applying a `negative VID offset` we lower the `[VID]`, and therefore the `[VCORE]` (the actual voltage the CPU gets from the VRMs).
F.a. if your `[VID table]` says 1.300 V for 5.5 GHz, and you apply a negativ offset of 0.120, the requested `[VID]` will be 1.300 - 0.120 = 1.180 V.

### ASUS

### MSI

### GIGABYTE


