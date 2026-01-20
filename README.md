# Multi GPU system story

## Purpose
Large AI models require alot of VRAM. The original purpose of this system was to run LLM models up to 70b parameters with Q4_K_M quantization (4.83bpw), which is the "sweet spot" for local LLMs..

## Part 1
Initially, I had RTX 3090 gpu. The power supply I had at the time (1000 W) had plenty of headroom to spare, so I added an rtx 4070 from a prebuilt just to see what would happen.

![Pic 1](/images/1.jpg)

This setup allowed me to learn more about tensor-splitting, and it allowed me to load larger LLM models. The 4070 was slowing down inference slightly, but I could now load models up to 36b parameters at 5 bit quantization and plenty of context length. Interestingly, I could load a 70b LLM at 3bit quantization and a small context window, but it's not very useful.

I decided to go deeper down the rabbit hole...

![Pic 2](/images/2.jpg)

Another 3090 gave me 48 GB vram, which is enough for my original purpose. I could now run inference on a Q4_K_M quant of 70b LLM, albeit with a small context window.

It's not all roses, however. Two GPUs running 350 W produces a lot of heat, so thermals were a major concern with the old PC case I had at the time.

![Pic 3](/images/3.jpg)

## Part 2

Despite using power limits and undervolting, the concerns about temperatures were always in the back of my head. They never exceeded 70° celcius during during inference. This is because inference is generally more memory-bound rather than compute bound. This machine could not be used for long AI training.

The issue was the lack of space between the 2 gpus, and the poor cooling and airflow of the old NZXT case. The motherboard has three PCIe slots and a single x1 PCIe slot, but due to the case profile I could not insert the second GPU into the lowest slot.

It was time for an upgrade...

![Pic 4](/images/4.jpg)

![Pic 5](/images/5.jpg)

The new case has plenty of room to work with and supports up to fifteen 140 mm fans. The case upgrade alone helped the thermal issue massively. The final product worked flawlessly.

![Pic 6](/images/6.jpg)

## Part 3

Working with context of 16,000 tokens only gets you so far... The VRAM ceiling hit fast and hard, and the rabbit hole only became deeper.

The motherboard had two unoccupied PCIe slots. Theoretically, I could add two more GPUs into this system. However, I decided to start small with a low-power RTX 3060 GPU just to see if the PC would work with three graphics cards in.

Inserting it wasn't as simple as it seems. Due to other case and fan cables at the bottom of the motherboards circuit, the GPU would physically not fit in. Riser cable was my first solution to this problem.

In the setup below, the top 2 GPUs are attached directly into the motherboard and the bottom 3090 GPU is attached with a long riser cable.

![Pic 7](/images/7.jpg)

The RTX 3060 was a nice VRAM buffer, but it's very slow compared to the 3090. After confirming that the PC would boot with three GPUs, I decided to go all out and get a third 3090 GPU. However, RTX 3090 has 350 W stock TDP, and even with powerlimiting and undervolting, the old PSU did not have enough PCIe power cable slots and power spikes could still happen.

Soon after, I got a good deal on an used 3090 Ti, which has TDP of 450 W. I knew I would need a bigger power supply to make this work, so I got a 1500 W one.

However, this is where the jank begins. It's difficult to fit three 3-slot graphics cards into a regular consumer motherboard due to the PCIe slot spacing. In order to fit the third 3090 in, I would need two PCIe risers.

![Pic 8](/images/board.jpg)

I decided to go with a small riser card to push the middle GPU slightly forward, in order to make room for the long riser cable for the bottom GPU. See the rough sketch below of the current layout:

![Pic 9](/images/layout.jpg)

![Pic 10](/images/9.jpg)

Despite the tight spacing, the new case has much better airflow. The case includes an internal fan bracket to push air between the GPUs with high pressure.

![Pic 10](/images/8.jpg)

## Hardware list

| Part          | Part name     |
| ------------- |:-------------|
| CPU           | Ryzen 5800x3D |
| Motherboard   | Asus ROG Crosshair VIII Dark Hero x570     |
| Cooler        | Noctua NH-D15     |
| GPU 0         |  RTX 3090 Asus TUF OC |
| GPU 1         |  RTX 3090 Ti Gainward Phantom |
| GPU 2         | RTX 3090 Founders Edition |
| Power Supply| Corsair HX1500i |
| Riser card    | DeLock PCIe x16 -> x16 riser |
| Riser cable | Lian Li PCI-e 4.0 x16 riser cable |
