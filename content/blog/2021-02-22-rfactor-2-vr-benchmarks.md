---
title: rFactor 2 VR Benchmark
date: Monday, February 22nd, 2021
thumbnail: /images/uploads/benchmark.webp
category: Sim
---
**rFactor 2 DevMode used to gain some insights on VR performance**
### Methodology
These benchmarks where created with an early implementation of an automated
rFactor 2 specific benchmark tool. I've not yet published the code for it,
because it is very specific to work with fpsVR and attempts to get
[Nvidia's Fcat VR](https://www.geforce.co.uk/hardware/technology/fcat/downloads) 
to run on my system failed so far.

The future plan is to integrate the [rF2 Settings Widget](/page/rF2-settings-widget)
capabilities to run automated benchmarks with a variety of content.
The current app relies on the DevMode executable which has limited content aboard.
Therefore, you will not find track and car specific benchmarks like Circuit
and Vehicle Detail.

The goal is not to have some numbers saying this system is capable or not,
but to have an insight how much relative CPU or GPU frametime one would lose when
activating certain settings.

### CPU Single Thread Limit
First and foremost it needs to be said that the update rate of this Sim is
limited by its physics engine. If you don't throw enough single thread 
performance at it, it will perform somewhat unpredictable whenever 
physics ticks can not be delivered in time.

One can read quite a lot of comments that game X is only utilizing one
or two CPU cores. It is not that simple.
Even today, it is not an easy task to calculate 
across multiple threads efficiently, if you need to consider concurrent 
results from other worker threads.

Now throw 4 smart engineers at this problem, with smart I mean expensive,
and you will have a feature that people will take for granted but will
not pay a single extra Euro for. So it is not surprising to find this 
behaviour in most non-multi-million dollar realtime engine productions.

rFactor 2 is no exception to that so this information is only really
useful if the machine it is running on is beyond that single thread bottleneck.

#### Optimise CPU usage
In any case one can free the CPU from load by reducing the amount of draw
calls and geometry. Thus setting Circuit, Vehicle and Opponent
Details to the lowest possible setting.

## First Benchmark Results
**System**
 - i7-5820k @ 4,2GHz, 32GB DDR4-2888
 - GeForce GTX 1080 Ti
 - 1TB Samsung 860 EVO SSD (serving only the game not the OS)
```
Rain-Drops
       GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off        13.870241       8.234471          NaN          NaN
Low        14.440307       8.753541     4.109991     6.303620
Ultra      16.333206       8.787850    13.108447     0.391947


Rearview-Particles
     GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off      13.802886       8.524134          NaN          NaN
Low      14.258536       8.647248     3.301119     1.444302


Road-Reflection
       GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off        13.823519       7.812769          NaN          NaN
Low        13.870241       8.234471     0.337987     5.397606
Ultra      14.708351       8.574477     6.042506     4.129054


Environment-Reflection
      GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off       13.968590       8.414098          NaN          NaN
Low       13.870241       8.234471    -0.704076    -2.134825
High      14.341655       8.601035     3.398747     4.451571


Soft-Particles
      GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off       13.870241       8.234471          NaN          NaN
High      13.950171       8.446366     0.576269      2.57326


Special-Effects
       GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off        14.204147       8.522965          NaN          NaN
Low        14.230783       8.482059     0.187522    -0.479947
Ultra      14.330645       8.707881     0.701732     2.662349

# i9-10850k + RTX3090, Results are somewhat sluggish

Post-Effects
        GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off          2.690852       4.291171          NaN          NaN
Low          2.586419       4.095671    -3.881025    -4.555859
Medium       2.998736       4.559566    15.941603    11.326478
High         3.403225       4.346985    13.488658    -4.662305
Ultra        5.619619       4.366498    65.126272     0.448883


Shadow-Blur
        GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off          2.756997       5.123894          NaN          NaN
Low          2.795348       4.575426     1.391046   -10.704114
Medium       2.807449       4.579574     0.432884     0.090649
High         3.042394       5.216947     8.368650    13.917747


Shadows
        GPU Frametime  CPU Frametime  GPU Percent  CPU Percent
Off          2.724962       4.546595          NaN          NaN
Low          2.687672       4.593158    -1.368449     1.024136
Medium       2.792317       4.598825     3.893516     0.123370
High         2.798490       4.700067     0.221047     2.201479
Ultra        3.052708       4.686365     9.084123    -0.291515

Process finished with exit code 0
```

## Conclusion
It is quite interesting that you can gain 21% of GPU time alone by 
leaving Reflection settings at Low and Rain Drops set to Off.
Other big GPU hits will be Multi Sample Anti Aliasing, super 
sampling factor, the amount of visible vehicles when headlights are
active and Post Processing. These will have to wait for a later date.

**I will try to update this page whenever I find the time to further
develop the benchmark tool**
