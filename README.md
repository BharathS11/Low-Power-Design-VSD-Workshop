# **Low Power Design Workshop By Srikanth Jadcherla and VLSI Systems Design**

One of my professors used to say "Make either a big chip or a fast chip but never a bigger and faster chip". If its both big and fast it will generate lot of heat and probably fail. Pentium 4 "Prescott" is classic example (almost 3-4x more transistors, increased die size and frequency aimed to achieve 4GHz), consumed 100 - 150 Watts of leakage power <sup>(1)</sup>. 

Until a year ago I used to undervolt my CPU by about 115mV and that would give me constant performance of 4.0+ GHz, without undervolting I would run into thermal throttling issues. Essentially it will decrease the dynamic power, enabling cooler temperature and better performance. The need for Low power systems is evident from this. Unfortunately because of the Plundervolt bug, Intel and Dell decided to block that feature and thus my CPU thermal throttles.

We are heading towards handheld devices, for them to be efficient we need to optimize power consumption. The workshop provided me with a basic understanding of various techniques used to achieve Low power consumption and verification of those said techniques.

>## Workshop Curriculum
> ### Module 1 : Introduction
> 1. Why Low power design?
> 2. Low Power Fundamentals
> 3. CMOS Recap
> ### Module 2 : 
> 1. Low Power Implementations
> 2. Voltage Control Techniques
> ### Module 3
> 1. State space Basics
> 2. Multi-Voltage Terminology
> ### Module 4 
> 1. Voltage aware booleans
> 2. Power management and Typical Errors
> 3. Verification strategies of MV designs
> ### Module 5
> 1. Island Ordering
> 2. Mobile and Mobility

</br>
</br>


> ### CMOS Basics
Early microprocessors like Intel 4004 were pure NMOS design. Lack of complementary devices (PMOS and NMOS) in a pure NMOS tech made realization of inverters with zero static power non-trivial. Hence, in 1980s shift to CMOS was made. CMOS are voltage controlled devices, primary reason to be used in Low power design</br>
![CMOS Basics](/images/cmos_basics.PNG)

|        |State of Design Elements||
|        |Combinational Logic| Sequential Logic|
|--------|-------------------|-----------------|
|Shutdown|OFF|OFF|
|Standby |ON |Reads allowed but not writes|
|Active  |ON |Reads and Write allowed|

</br>

> ### Power
 
Power consumed by an SOC implies the switching power proportional to the activity and static power, also known as Leakage power.

![Power Consumed](/images/power_d_st.PNG)</br>
**Dynamic power:** Power required to charge and discharge the output capacitance. V<sub>DD</sub> decreases with every node, should reduce dynamic power, but increase in number of transistors negates the reduction in V<sub>DD</sub>.

**Static Power /Leakage Power:** Power consumed when idle, it could be because of various factors <sup>(2)</sup>
1. Subthreshold Current
2. Drain induced Barrier Lowering
3. Punchthrough
4. Thin Oxide Gate Tunneling
5. Gate Induced Gate Leakage
6. PN Junction Current
7. Hot Carrier Injection

**Energy** is power consumed per unit time, it's the capacity that is packed. Directly affects weight and cost, indirectly affects form factor. So how much is enough?

**Power Management** is often the problem of Density, Delivery, Leakage and Lifetime
1. Density: is power consumed in unit area, meaning heat generated, meaning cost required to dissipate heat. High density close to batteries has caused fires in cellphones and laptops.
2. Delivery: is the amount of current that needs to be delivered. As Vdd decreases current delivery increases. Hence, delivery is commonly known as IR drop and di/dt problems.
3. Leakage: is the power consumed when idle. In higher technology nodes this was insignificant, but in sub-micron and deep sub-micron designs because of second order effects this is a significant problem. 
4. Lifetime: is reliability problem because of ever shrinking technology node. The chip cannot fail because of this, designer has to ensure reliable operation for defined period.

</br>

> ### Case Study
> 1. Thermal Runaway - Why Samsung phones caught fire?
> 2. Cost and Characteristics of Fans Mounted on CPU
> 3. Iphone 6 Battery Degradation Law suit

</br>

> ## Multi - Voltage Terminology
**1. Rail:** A Virtual or Physical network connected to the output of a Voltage Source.</br>
**2. Island:** is a set of logic elements with 7 rail connections. </br>
**3. Well:** is a set of cells with common bulk connections for both PMOS and NMOS.</br>
**4. Domain:** is the drain of the driver. It indicates which primary rails are driving the signal.</br>
**5. Spatial Crossing:** is a signal sourced in one island and has a destination in another.</br>
**6. Temporal Variation:** is the variation of rail over time.</br>
**7. Isolation:** is a technique to protect a receiving island that is active from a signal originating in an island that is turned off. </br> <img src="images/isolation_parking.PNG" width="300px" height="150px"></br>
**8. Input Isolation/ Parking:** is a technique to arrest input toggles in an island.</br>
**9. Level Shifter:** is a technique to convert a signal driven by one set of primary rails to another set of primary rails on a spatial crossing.</br>![Level Shifters](/images/level_shifter.png)</br>

</br>

> ### Voltage Control Techniques
1. 7 Degree of voltage control
2. DVFS, Power Gating, Retention and VDD standby
   
There are only 7 ways the CMOS voltage can be controlled, it's as shown below</br>
<img src="images/volatge_control.png" width="400px" height="400px"></br>
By varying any of these, applied voltage thus the output voltage level can be controlled.

**Power Management Techniques**
1. **Clock Gating:** Clock trees are responsible for a significant portion of dynamic power. The most common way to reduce this is to switch off the clocks when not in use. This technique is called Clock Gating.</br>![Clock Gating](/images/clk_gating.PNG)
2. **Multi-threshold:** Leakage depends exponentially on V<sub>TH</sub>. Many PDK have multi-Vt cells. The idea is to replace non-critical path cells with slow, less leaky High V<sub>TH</sub> cells. There by minimizing the leakage. </br> ![Mutli Threshold](/images/multi_vt.PNG)</br>
3. **Multi-Voltage:** Dynamic power is proportional to V<sub>DD</sub><sup>2</sup>. Hence, lowering V<sub>DD</sub> decreases dynamic power significantly, but also makes the gates slower. </br>
Consider an example, Timing critical components like cache can be run at max voltage, the CPU can be run at a slightly reduced voltage and other non-critical components can be run at further reduced voltage. This will provide significant power reduction.</br> 
![Multi VDD](/images/multi_vdd.PNG)
4. **Power Gating:** Leakage power is increasing with new technology node, since we are moving towards more efficient hand held devices the need for reducing leakage power is higher. One of the ways to achieve that is to switch off the block that are not in use. This technique is called Power Gating
5. **Power Gating with State Retention:**
6. **Dynamic or Adaptive Voltage Frequency Scaling:**
7. **Low VDD Standby:**
> ### State Space and Retention

> ### Voltage Aware Booleans
> ### Power Management Bugs
> ### Verification Strategies of MV Design




References:
1. [The Quest for More Processing Power, Part One: "Is the single core CPU Doomed?"](https://www.anandtech.com/show/1611)
2. [Leakage and Low Power, R Saleh, UBC Canada](https://courses.ece.ubc.ca/579/579.lect6.leakagepower.08.pdf)


