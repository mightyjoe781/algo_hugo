+++

Title = "General Issues"
weight = 7

++++

#### Clock Issue

So Actually each hardware has a clock on its motherboard that keeps track of time even though computer is off.

Windows assumes the time is stored in local time, while Linux assumes the time is stored in UTC time and applies an-offset. This leads one of OS show incorrect time.

To solve this problem there are 2 options

- Disable RTC in Linux
- Use UTC in Windows.

Follow only one of instruction

1. ##### Disable RTC on Linux

   ````bash
   timedatectl set-local-rtc 1
   ````

2. ##### Use UTC in Windows

   Go into Date and Time Settings

   And turn off **Set time automatically.**

<hr>



