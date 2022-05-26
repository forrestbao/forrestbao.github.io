# Z690 motherboards for multi-GPU workstaions

I am now selecting a Z690 motherboard for my multi-GPU worktation. The model number Z690 is because it is the best chipset (among the 600 series) supporting Intel 12th generation Core CPUs. In terms of GPUs, I have 3 3090's. After hours of research, I am writing this blog to save my time in the future. 

First, no Z690 (or even the entire 600 series) motherboard can actually support more than 4 PCIe slots, except [Asus Prime Z690-P D4](https://dlcdnets.asus.com/pub/ASUS/mb/LGA1700/PRIME_Z690-P_D4/E18744_PRIME_Z690-P_D4_UM_WEB.pdf). But one of its PCIe 3.0 slot is not physically accessible when the "main" PCIe 5.0 slot, directly from the CPU, is in use. One possible solution is to connect 3 GPUs using PCIe extension cables. But that would make things too complicated. Hence, I give up the plan of building a quad-GPU workstation. 

Second, many triple-slot Z690 motherboards have this PCIe configuration: 
* x16 PCIe 5.0 (directly from CPU)
* x4 PCIe 4.0 (from Z690 chipset) -- although physical slot is x16
* x4 PCIe 3.0 (from Z690 chipset) -- although physical slot is x16

In contrast, some advanced motherboards split the PCIe5.0 x16 lanes directly from the CPU into two PCIe5.0 x8 lanes, resulting in the configuration below for triple GPUs: 
* x8 PCIe 5.0 (directly from CPU) -- although physical slot is x16
* x8 PCIe 5.0 (directly from CPU) -- although physical slot is x16
* x4 PCIe 4.0 (from Z690 chipset) -- although physical slot is x16
An illustration is [here](https://download.msi.com/archive/mnu_exe/mb/MEGZ690ACE.pdf#%281%297D27v1.0-EN.indd%3A.177192%3A554) or [here](https://download.asrock.com/Manual/Z690%20Taichi.pdf#page=32). 

I think this is better than the first configuration because the speed and bandwidth differences between the three slots, especially the first two, are not big. On such motherboards, you usally can see two or three emhanced PCIe slots, like [this](https://c1.neweggimages.com/ProductImageCompressAll1280/13-144-505-V04.jpg) or [this]((https://c1.neweggimages.com/ProductImageCompressAll1280/13-119-504-V02.jpg). 

Among the candiates that support this configuration ([selecting two PCIe5.0 x16 and one PCIe4.0 x16](https://www.newegg.com/p/pl?N=100007627%208000%204131%20601394305%20601394886%20601361046&Order=1)), Asrock's Z690 Tachi seems to be cheapest.

However, I am still keeping an eye on dual-GPU configurations because I can have a 4-slot spacing between the GPUs and two GPUs can be mounted on an open-air case easily, such as [this one](https://www.newegg.com/gigabyte-z690-aorus-elite-ax-ddr4/p/N82E16813145350) or [this one](https://www.newegg.com/gigabyte-aorus-z690-ud-ddr4/p/N82E16813145349). 


So, to be updated...
