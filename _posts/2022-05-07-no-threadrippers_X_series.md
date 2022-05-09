---
layout: post
title: "Demystifying PCIe lanes in GPU computing for Deep Learning: Forgetting about AMD Threadrippers and Intel X-series"
date: 2022-05-07
author: Forrest Sheng Bao/鮑盛
---

* I am building a multi-GPU workstation of my own. 
* I previously owned an Intel i9 10980XE CPU. 
* And I am here to tell you that the 48 PCIe lanes of Intel X-series CPUs and the 128 lanes of AMD Threadripper CPUs are overkills for multi-GPU computers despite that the GPUs can be most effective with 16 PCIe lanes each. 
* Just get a regular desktop CPU from Intel or AMD (with Intel's perferred), paired with a proper chipset. 
 
--- 

GPUs talk to CPUs via a type of parallel data bus called the PCIe. 
The number of bits that can be transmitted each time is called the number of PCIe lanes. 
Most modern GPUs support up to 16 lanes. 
So if you have three 3090s, ideally, 3x16=48 PCIe lanes are needed from the CPU. 

However, most consumer-level CPUs, Intel's or AMD's, have only 20 PCIe lanes, 16 for one GPU and 4 for one M2 SSD.
And such 16 lanes are only wired to one PCIe slot, usually the top-most slot closest to the CPU. Thus, only one GPU can be connected to the CPU and fully use the PCIe bandwidth. One solution is using CPUs of more native PCIe lanes, such as AMD Threadrippers offering up to 128 PCIe lanes or Intel X-series offering 48 PCIe lanes -- and your motherboard manufactuer needs to fan out the 128 or 48 lanes to 8 or 4 PCIe slots. 

Well, this solution, expensive and hasslesome, is an overkill. The chipset paired with a CPU also branchs out some PCIe lanes from the CPU. Take a look at the block diagram of Intel Z690 chipset below. In addition to the 20 CPU-native PCie 5.0 lanes, the chipset provides 12 PCIe 4.0 lanes and 16 PCIe 3.0 lanes. That's where additional GPUs can use to talk to the CPU. 

![Intel 12th-generation Core and Z690](https://www.servethehome.com/wp-content/uploads/2021/10/Intel-Z690-chipset-block-diagram.jpg)

But, the additonal GPUs will have to share the chipset-provided PCIe lanes. And how they share depends on how the chipset-provided PCIe lanes are wired to physical PCIe slots. 
For example, the 16-channel PCIe 3.0 lanes can be shared between two PCIe slots with each slot getting 8 lanes. 
Or, they can be shared between 4 PCIe slots with each slot getting 4 lanes. 

Now you probably start to worry about the speed. 
Not only does the chipset-enabled PCIe lanes are slower (4.0 and 3.0 here, vs. 5.0 of the CPU-native ones), they are also shared among GPUs. On top of that, the bandwidth between the chipset and the CPU is also mingled with other I/Os. For example, the data exchange rate for a GPU that is sharing the chipset-provided 16 lanes of PCIe 3.0 with another GPU is at least 8 times slower than that for a GPU directly connected to the CPU using 16 lanes of PCIe 5.0. (Math: PCIe 5.0 is 4 times faster than PCIe 3.0 and 16/8=2. )

Luckily, it's not a big deal. Tim Dettmers talked about it in [2018](https://timdettmers.com/2018/12/16/deep-learning-hardware-guide/) and [2020](https://timdettmers.com/2020/09/07/which-gpu-for-deep-learning/#Do_I_need_8x16x_PCIe_lanes) that the PCIe bus speed or width impacts deep learning speed slightly. 

**Therefore**, you do not need a server-level CPU or an extreme consumer-level CPU. A midrange one between $100 and $500 should be enough for you. But since all GPUs but one uses the PCIe lanes from the chipset, do get a motherboard with a powerful chipset. For example, the cost of an Intel Z690 motherboard is just $200. Unless you really wanna save $50 or $70, you can opt for H670 offering 12 PCIe 3.0 lanes or B660 offering 8 PCIe 3.0 lanes. A comparison between Intel 600 series chipsets is below. 

![comparison of Intel 600 series chipset](https://i.pcmag.com/imagery/articles/05hN54AraDsxVA1mw8k5rJE-4.fit_lim.size_1344x.jpg). 

## Addtional reasons against Threadrippers and X-series

Intel's X-series are only up until 10th-generation i9. The PCIe standard it supports is PCIe 3.0. This CPU's TDP is 165W. 

AMD's Threadrippers, have a crazy TDP, 280W. This will add up the thermal and power issues of a (multi)-GPU workstations.

## So Intel or AMD?

Intel, for sure. 
* CPU-native PCIe lanes: Both Intel's and AMD's regular CPUs have 16 PCIe lanes for the first GPU. But Intel's is PCIe 5.0 while AMD's is PCIe 4.0. 
* chipset-provided PCIe lanes: Intel's Z690 has 12 PCIe 4.0 lanes and 16 PCIe 3.0 lanes. In contrast, AMD's X570 chipset has only 8 PCIe 4.0 lanes. The main reason is because Intel's 12th-generation Core CPUs talk to Z690 via a DMI bus while in the case of AMD, it's PCIe 4.0. 

![AMD Ryzen Gen 3 and X570](https://i.imgur.com/8Aug02l.png)
