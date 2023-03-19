# Fan Protector
Simple polulu-compatible protection board for Xiaomi/Roborock radial fans

### A word about all that hustle
>Remote cooling system are becoming a trend in scope of hi-end 3d printers. You mount big and powerful turbine somewhere on the printer and connect it to the toolhead with lightweight and flexible hose which you can get as a spare part for CPAP medeical machines.
>
>Some DIY projects utilize big 7040 blowers for that purpose. 7040 is a BLDC fan with external control board which is sold as a kit. Has pretty much power and cost around 50$ (which is quite steep for this kind of product imho)
>
>Turns out there are compact fans for robotic vacuum cleaners widely available on Aliexpress and other places as a replacement part. Provided in refurbished condition they cost five-times less than 7040 and still have comparable potential.

So if you want to adapt cheap (10$) and powerful vacuum cleaner turbines to your CPAP cooling system you can just connect PWM and DC to it... Most likely it will burn your electronics and finally set you free from 3D printing addiciton <br> **However, this type of connnection is strongly prohibited**, as it may (**will**) **fry** your **equipment** due to lack of certian passive components which is expected to be at the motherboard side.

For more details about XiaoFans aka Roborockers, please refer to <a href="https://github.com/condottab/Roborock-CPAP" target="_blank">this repo from a friend of mine</a>

My take on that situation
---
So, to make use of XiaoFan good folks designed a simple schematic which adds missed protection features and stabilizes control voltage. To simplify installation process and eliminate wiring "fun" I designed that simple polulu-compatible protection board, based on original schematic.

<figure>
    <img src="/readme_pics/polulu_installed.jpg"
         alt="protector installed in polulu socket">
    <figcaption>As description suggests, this protector can be installed right in the driver socket of motherboard</figcaption>
</figure>

Once the board installed, the corresponding 4pin XH2.54 motor connector become the protected fan connector. **It will use the same power source your steppers powered from, so keep that in mind.** The XiaoFan rated 15V max, but it can be overvolted to 24V and tuned down to ~60% using PWM to continious run. 

> :pushpin: **Note:** This fan is a beast, so 60% of it will be already an overkill for some cases

However, if you reading this, you probably want to squezze all from XiaoFan setup, and that's won't be a problem. Just to be safe you can provide some sort of cooling to the cooling fan (haha). The 40mm fan can be a way to go (refer to link in the begining).

### Connecting to Klipper
Nothing too complicated, just add another fan section. You can find examples for Qinatsu and Nidec (indiacated at blower PCB) fans by using link above. 

* #### For polulu connection
you will need to know STEP and DIR pin adresses for driver slot, where Fan Protector is installed. STEP pin is serves as PWM and DIR serves as FG pin (tachometer, probably useless, and will be moved to another pin in Fan Protector v2.0 for Nidec workaround)

* #### For inline/"classic" connection
you will need to connect three (four with FG) pins: GND, VDD (aka VDD aka "plus") and PWM. The XiaoFans is pretty powerhungry (~2.5A at 24V 100%) so you should power it from main DC supply (PSU). PWM pin should be connected to any unoccupied PWM-controllable output on the motherboard. Ideally this pin should support hardware PWM, so you can go for higher frequencies to eliminate "whistle" caused by low-freq PWM. Klipper should warn you, if choosed pin is unsupported by hardware PWM. <br> *Also this type of connection is already described in repo by link above which is should be considered prior*

### Connecting to Marlin

Not yet tested/proved, but should be done with pin remapping according to scheme. TODO

---

### The Nidec case
As you can see, making use of this kind of tech comes with certain troubles. This time, it's Nidec fan tricky startup sequence. For some reason, they require PWM signal to be present when powering it up. In other words, PWM signal and DC power should be supplied together at the same moment. If not, Nidec won't start. Thankfully Klipper comes with *enable_pin* functionality, therefore we can just put a MOSFET in DC circut and tell Klipper about it.<br>
The only problem is my design doesn't have this MOSFET. Why, you ask? :eye:4:goat: ... Well, I guess that's what ver2.0 is for
> :pushpin: **Note:** As you can probably noticed, this design supports both polulu and standart connection. Let's consider standart connection a default variant by now. 

<figure>
    <img src="/readme_pics/inline_installed.jpg"
         alt="protector installed inline">
    <figcaption>Use the pins on the short sides for inline connection</figcaption>
</figure>
