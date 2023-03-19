# Fan Protector
Simple polulu-compatible protection board for Xiaomi/Roborock radial fans

### A word about all that hussle
>Remote cooling system are becoming a trend in scope of hi-end 3d printers. You mount big and powerful turbine somewhere on the printer and connect it to the toolhead with lightweight and flexible hose which you can get as a spare part for CPAP medeical machines.
>
>Some DIY projects utilize big 7040 blowers for that purpose. 7040 is a BLDC fan with external control board which is sold as a kit. Has pretty much power and cost around 50$ (which is quite steep for this kind of product imho)
>
>Turns out there are compact fans for robotic vacuum cleaners widely available on Aliexpress and other places as a replacement part. Provided in refurbished condition they cost five-times less than 7040 and still have comparable potential.

So if you want to adapt cheap (10$) and powerful vacuum cleaner turbines to your CPAP cooling system you can just connect PWM and DC to it... <br><mark>However, this type of connnection is strongly prohibited</mark>, as it may (**will**) **fry** your **equipment** due to lack of certian passive components which is expected to be at the motherboard side.

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

> :note: **Note:** This fan is a beast, so 60% of it will be already an overkill for some cases

However, if you reading this, you probably want to squezze all from XiaoFan setup, and that's won't be a problem. Just to be safe you can provide some sort of cooling to the cooling fan (haha). The 40mm fan can be a way to go (refer to link in the begining).



