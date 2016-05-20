---
layout: post
title: "First Test Post"
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


In order to make the *redesign* more interesting, i've added a couple features that weren't on the orignal design, but would have been very handy. 

* Provide a visual means for indicating the rails are regulated
* Provide a visual indication for battery charge remaining

---
<h3><center><b>Blown Fuse Indicator Circuit</b></center></h3>
![minipic](/img/reg/fuse_blow.jpg)

This circuit is slick in that the LED will only light when the fuse has opened.  In all other conditions the voltage drop across the fuse disallows the LED to light.  When sizing the LED current limiting resistor, a full 12V is assumed at then input of the LED. Note:This circuit only works if the load size of the resistor has a heavy constant load.  If there is no load, the LED will not light, as their is no return path for the current from the line side of the circuit.

---
<h3><center><b>Linear Regulator Circuit and Losses</b></center></h3>
![minipic](/img/reg/lin_regs.jpg)

Shown above, because of the nature of linear regulators there is a lot of extraneous current draw from the battery.  A conservative estimate of this waste is 1.9A.  

Since the robot was a heavy wheeled robot, the total current consumption was approximately 20A during operation (most of the current coming directly off the battery to the motor drivers).  Which meant waste current was only about 10% at worst.  

It is still apparent that for that project, based on time and budget constraints the LDO configuration was probably best.  However, I still wanted to explore the gains of a switching regulator design.  I decided to focus on the 7.4V @ 2A supply, with a 14-10V input only. 

There are many switching regulators/controller ICs out there.  One that caught my eye is the LT1771 from Linear Technology.  As with most of these ICs the datasheet comes with a typical application circuit, which is a great place to start.  

---
<h3><center><b>Original Application Note</b></center></h3>
![minipic](/img/reg/anote_orig.jpg)

The application circuit has a 3.3V output, but in our configuration we need a 7.4V output.  To accomplish this, we first change the resistor divider that feeds the feedback pin (FB).  



