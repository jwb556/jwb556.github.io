---
layout: post
title: "A Linear vs Switching Regulator Design Comparison"
date: 2016-05-19
---

# Regulator Do-Over
Re-exploring a design decision on project done in school

Switching Linear Regulators to switching on a robot project.

In school, I helped a ME student team with power regulation for a robot project they had already defined and purchased subcomponents for.  The team had overlooked the need for voltage regulation when purchasing their parts and very little budget left.  I was asked to design a circuit to take power from (2) lead acid batteries and provide (3) voltage rails.  

For sake of speed and cost I decided to use linear regulators.  The design worked, was easy to build, and was cheap to implement, but I always wondered about the actual gains I could have seen by implementing switching regulators. 

The actual design requirements are/were pretty short.

* Provide overload protection for the system
* Regulate input from (2) series connected lead acid batteries to:
    * 7.4V @ 2A
    * 5V @ 1A
    * 3.3V @ 100mA
* Provide indication of a blown fuse

---
<h3><center><b>Blown Fuse Indicator Circuit</b></center></h3>
![minipic](/img/reg/fuse_blow.JPG)

This circuit is slick in that the LED will only light when the fuse has opened.  In all other conditions the voltage drop across the fuse disallows the LED to light.  When sizing the LED current limiting resistor, a full 12V is assumed at then input of the LED. Note:This circuit only works if the load size of the resistor has a heavy constant load.  If there is no load, the LED will not light, as their is no return path for the current from the line side of the circuit.

---
<h3><center><b>Linear Regulator Circuit and Losses</b></center></h3>
![minipic](/img/reg/lin_regs.JPG)

Shown above, because of the nature of linear regulators there is a lot of extraneous current draw from the battery.  A conservative estimate of this waste is 1.9A.  

Since the robot was a heavy wheeled robot, the total current consumption was approximately 20A during operation (most of the current coming directly off the battery to the motor drivers).  Which meant waste current was only about 10% at worst.  

It is still apparent that for that project, based on time and budget constraints the LDO configuration was probably best.  However, I still wanted to explore the gains of a switching regulator design.  I decided to focus on the 7.4V @ 2A supply, with a 14-10V input only. 

There are many switching regulators/controller ICs out there.  One that caught my eye is the LT1771 from Linear Technology.  As with most of these ICs the datasheet comes with a typical application circuit, which is a great place to start.  

---
<h3><center><b>Original Application Note</b></center></h3>
![minipic](/img/reg/anote_orig.JPG)

The application circuit has a 3.3V output, but in our configuration we need a 7.4V output.  To accomplish this, we first change the resistor divider that feeds the feedback pin (FB).  

With an input 7.4V i need an output of 1.23V to feedback into the feedback port.  Solving the standard voltage divider equation gives R2/R1 = 1.23/(7.4-1.23).
This means the ratio is .199 or approximately .2.
For sake of using readily available resistors, R3 will be 1Meg and R4 will be 200k.

Switching these two resistor values only yields the following simulation:

<h3><center><b>Feedback Resistor Change Only</b></center></h3>
![minipic](/img/reg/mod1_sim.JPG)

Changing only those two feedback resistor yielded large steady state error, 6.8V out instead of 7.4V out.  To compensate for this the datasheet suggests increasing the inductor and output filter capacitor values.  Changing the inductor value from 15uH to 60uH and the output capacitor from 150uF to 300uF seemed to do the trick in simulation.


<h3><center><b>Feedback Resistor and Inductance/Capacitance Increase</b></center></h3>
![minipic](/img/reg/mod2_sim.JPG)

When measured in this configuration the output voltage was 7.35V.  Since this rail was originally for hobbyist grade servo motors, this 1.4% error is close enough.

This is a great result, but to be slightly more thorough, I wanted to simulate esr of both the capacitor and main inductor.  To start I found parts on Digikey with similar values to extract series resistance from.

* Inductor (ferrite core) Wurth PN:7447709680, 68uH 3.2A rating, 3.6A current saturation, 89mOhm series resistnace.
* Cacitor (electrolytic) Nichicon PN:UCZ2A301MNQ1MS, 300uF, 100V rating (excessive), 110mOhms at 100kHz (this regulators switching speed)

With these parameters included in the simulation, the output voltage is now approximately 7.335V, which is 6.5% error, but again would be close enough for power rail of hobbyist servos.

If this were unnacceptable, the output voltage could be easily trimmed by including a potentiometer, configured as a rheostat across R4.  This design would now satisy the requirements I had for the design in school, but because of cost and time constraints the linear regulator won out.

According to the simulation with an approximate 2A current draw on the output, had an input power of 17.44W, and an output power of 16.27W.  This waste wattage is approximately 1.17W, which could be roughly converted to 97mA of waste current.  This siginficantly better than the 1.3A of waste current draw from the battery in the linear configuration.

Bottom line: If the design required a smaller, more power conscious design, a regulator design such as the one here would have worked well.

---
