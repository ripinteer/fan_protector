# Fan Protector
Simple polulu-compatible protection board for Xiaomi/Roborock radial fans

### A word about all that hustle
>Remote cooling systems are becoming a trend in the field of high-end 3D printers. You install a big and powerful turbine somewhere on the printer and connect it to the tool head using a lightweight and flexible hose that you can purchase as a spare part for CPAP medical devices.
>
>Some DIY projects utilize big 7040 blowers for that purpose. 7040 is a BLDC fan with an external control board which is sold as a kit. Has pretty much power and costs about 50$ (which is quite steep for this kind of product imho)
>
>Turns out there are compact fans for robot vacuum cleaners, widely available on Aliexpress and other places as a spare parts. Provided in the refurbished condition they are five times cheaper than 7040, and still have comparable potential.

So if you want to adapt cheap (10$) and powerful vacuum cleaner turbines to your CPAP cooling system you can just connect PWM and DC to it... ~~Most likely it will burn your electronics and finally set you free from 3D printing addiciton~~ <br> **However, this type of connnection is strongly prohibited**, as it may (**will**) **fry** your **equipment** due to lack of certian passive components which is expected to be at the motherboard side.

For more details about XiaoFans aka Roborockers, please refer to <a href="https://github.com/condottab/Roborock-CPAP" target="_blank">this repo from a friend of mine</a>

My take on that situation
---
So, to make use of XiaoFan good folks designed a simple schematic that adds missed protection features and stabilizes control voltage. To simplify the installation process and eliminate wiring "fun" I designed that simple polulu-compatible protection board, based on the original schematic.

<figure>
    <img src="/readme_pics/polulu_installed.jpg"
         alt="protector installed in polulu socket">
    <figcaption>As description suggests, this protector can be installed right in the driver socket of motherboard</figcaption>
</figure>

Once the board is installed, the corresponding 4pin XH2.54 motor connector becomes the protected fan connector. **It will use the same power source your steppers powered from, so keep that in mind.** The XiaoFan is rated 15V max, but it can be overvolted to 24V and tuned down to ~60% using PWM to continious run. 

> :pushpin: **Note:** This fan is a beast, so 60% of it will be already overkill for some cases

However, if you reading this, you probably want to squeeze all from the XiaoFan setup, and that's won't be a problem. Just to be safe you can provide some sort of cooling to the cooling fan (haha). The 40mm fan can be a way to go (refer to the link in the beginning).

### Connecting to Klipper
Nothing too complicated, just add another fan section. You can find examples for Qinatsu and Nidec (indicated at blower PCB) fans by using link above. 

* #### For polulu connection
you will need to know STEP and DIR pin addresses for the driver slot, where Fan Protector is installed. STEP pin serves as PWM and DIR serves as FG pin (tachometer, probably useless, and will be moved to another pin in Fan Protector v2.0 for Nidec workaround)

* #### For inline/"classic" connection
you will need to connect three (four with FG) pins: GND, VDD (aka VDD aka "plus"), and PWM. The XiaoFans is pretty powerhungry (~2.5A at 24V 100%) so you should power it from the main DC supply (PSU). PWM pin should be connected to any unoccupied PWM-controllable output on the motherboard. Ideally, this pin should support hardware PWM, so you can go for higher frequencies to eliminate "whistle" caused by low-freq PWM. Klipper should warn you if the chosen pin is unsupported by hardware PWM. <br> *Also this type of connection is already described in the repo by the link above which should be considered prior*

<figure>
    <img src="/readme_pics/inline.jpg"
         alt="protector board with XH2.54 connectors installed">
    <figcaption>protector board with soldered XH2.54 connectors</figcaption>
</figure>

### Connecting to Marlin

Not yet tested/proved, but should be done with pin remapping according to scheme. TODO

---

### The Nidec case
As you can see, making use of this kind of tech comes with certain troubles. This time, it's Nidec fan tricky startup sequence. For some reason, they require a PWM signal to be present when powering it up. In other words, the PWM signal and DC power should be supplied together at the same moment. If not, Nidec won't start. Thankfully, Klipper comes with `enable_pin:` functionality, therefore we can just put a MOSFET in the DC circuit and tell Klipper about it.<br>
The only problem is my design doesn't have this MOSFET. "Why?" you ask.   :eye:4:goat: ... Well, I guess that's what ver2.0 is for <br>
**However!** This only means that **Nidec XiaoFans aren't supported by polulu configuration, but classic connection scheme is still viable.** You can power the fan through the one of exsiting heater MOSFETs on the motherboard, like additional heater port, which should be capable of driving 2.5+ Amps continuously. Set it's pin as `enable_pin:` in Klipper, connect the PWM and you should be good to go.
> :pushpin: **Note:** As you can probably notice, this design supports both polulu and standart connection. Let's consider standart connection a default variant by now. 

<figure>
    <img src="/readme_pics/inline_installed.jpg"
         alt="protector installed inline">
    <figcaption>Inline connection example</figcaption>
</figure>
