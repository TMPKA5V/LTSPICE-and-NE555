# LTSpice and NE555
## Brief introduction to circuit simulation

Circuit simulation greatly simplifies, speeds-up and lowers cost of design of circuits. We might say that complex designs cannot be done without extensive simulation.
There are some tools for simulation and many of them are built around SPICE, some are also available at no cost. I’ve been using SPICE now and then with different names but the latest flavor for me is now Linear Technologies’ “LTSpice IV” :  it is freely downloadable from Linear Technologies’ website and works great.
The program  is great in all of the juicy aspects: ease of use, libraries can be easily added with new components and large has community support. Of course PSPICE models (that is, the description of the components necessary to simulate its behaviour) can be extended.
I’m not giving a tutorial here on LTSpice IV, there’s a lot around. Rather I’m using it to investigate how the now almost 40-years-old NE555 works. I’ll also show (on a next post) how a real NE555 can be made into a building block for a better sawtooth generator.

I did not make use of a ready-made NE555 model available, rather I designed the NE555 schematic from single macro-components to gain access to the electrical nodes inside the component.

You might just want to look at the pictures and follow the explanations or actually follow me step-by-step on your own schematic.

![](/Images/1.jpg)

NE555 principle schematic is inside the dotted box. The “P<x>-<description>” ‘s are the connection to the pins of the 555.

I did not include power supply pins and nets for better readability.

A few hints before, in case you want to experiment and change some values.

The ground (the small triangle) net is the reference used by SPICE. It must be there somewhere connected to ground net, even if ground net were entirely drawn.

Positive supply is given a name (+V)  with the menu edit->Label net, It can be any name. You can change names right clicking on them but don’t do so, for now.

Move the mouse pointer next to +V (any label named so), right click and select “Highlight Net” to show that same labeled nets are connected through same names.

Most components’ values can be edited right clicking on them : try it with V1 or a resistor.  Some others you can change PSPICE models.

Right click on the transistor : a window will pop-up, select “pick new transistor” to display a selection of ready-made transistor you could select. Click cancel, for now.

Components can be added with “Edit->Component” or the icon on top, towards right (the logic “AND” shape).

This for the editor. The Internet is totally flooded with tutorials on LTSpice IV, just google for it and pick the best for you.

The circuit is the almost discrete version of the 555 connected as an astable multivibrator. The parts within the dotted box are the 555 itself, the components outside are the passives necessary to make the multivibrator.
The 555 includes a resistive network (R1, R2, R3) to create voltage references at 1/3Vcc and 2/3Vcc.
These references go into two comparators (U1, U2), I picked two general purpose from the ones present in the library (Edit->Place Component” then browse the “Comparators” subdirectory).

The outputs of the comparators drive the Set and Reset inputs of a SR Flip Flop. A SR Flip-Flop is a digital device whose output Q goes to logic 1 (in our case it goes to V+) if Set input is place to 1 (i.e. V+) and stays so even if Set input is placed back to Ground (i.e logic 0). Q remains set to V+ until Reset input is placed to 1 in place.

The outputs of the Flip Flop go to the output pin of the 555 and also make an open-collector current sink with the transistor. Q and Q negated are the outputs of the SR Flip Flop. Q negated has opposite behavior than Q but the transistor at Q negated reverts again the polarity so the net behavior is that both “P3-output” and the collector go to ground when the SR Flip Flop is reset, but only P3-Output” goes to V+ when SR Flip Flop is et to 1. The collector in this case remains “floating” and simply adapts itself to the voltage at the net “P7-Discharge” (the connecting point with R5 and R6).

I hope this sounds simple.

If you click on the schematic and then on the menu “simulate->Edit simulation cmd” a window will pop-up with the simulation configuration required. Click on the “Transient” top pane.
The values 9ms for “stop time” and 3ms for “Time to start saving data” should be typed in to define the time window that will be simulated and displayed.

At a later time you may want to change these values and see what happens.

Click OK.

IMPORTANT -  now the mouse might be attached to a rectangle that must be placed on the schematic somewhere. That string (.tran 0 9ms 3ms) defines the simulation we are going to do. When running the simulation (later on), if you get the message “No simulation found” or the window pops-up again, that’s because the string went lost.
  
  ![](/Images/2.jpg)
  
  You now may want to make the window full-screen.

If your mouse has a scroll wheel, point inside the schematic and scroll the wheel back and forth : in a few moment you should get lo learn how to zoom in and out different parts of the schematic. I love it, inherited from Autocad, I’d say, then Adobe picked that up.

Now click on the “Simulate->Run” menu or the icon with the running (or bad-smelling ?) man on i: a blank black window will appear above the schematic.

![](/Images/3.jpg)
  
Click again on the schematic window to give it focus.

From now on, the menu are contextual to schematic and simulation windows so, if you want to act on the schematic menu, click on the schematic window first. If you want to act on the simulation’s, click on it first.

Time to see some waveforms: move the pointer close to “P3-OUT”, the output pin of the 555. A probe will appear. Click there.
A trace will be added to the schematic, the output waveform of the circuit.
  
 ![](/Images/4.jpg)
 
On the horizontal axis is time, on the vertical is the voltage at the pin or net under analysis. We are now going to see what makes that waveform, proceeding backwards towards the capacitor.

We know that the capacitor has something to do with it : move and click on the capacitor “upper” pin, the one not connected to ground. That node is called “P2-trigger”.
The waveform at the capacitor is now superimposed to the output. As correctly assumed the output has a relation with charge and discharge ramps of the capacitor.
  
![](/Images/5.jpg)

For now we don’t need the capacitor voltage: press and hold “ctrl” on the keyboard and click again on the capacitor pin to let the trace go.
Now click inside the waveform window to give it focus, right-click and select “Add plot pane” to create a new plotting area above the first one.
Click inside the new plotting area, then click inside the schematic.
Add a probe (click on) to the “set” net inside the 555.
  
  ![](/Images/6.jpg)
  
You see, the output goes to V+ when “set” is pulled high to V+. For now we don’t care what makes the output go low to 0V (we may guess it is the “reset” net : click on it and look at the waveform. Then “ctrl” click again to remove it).
Now we have the output going low to 0V whenever “set” input of the SR Flip Flop is pulled high to V+.
The Set input of the Flip Flop is the output of a comparator: its output goes high (to V+) whenever its “+” input is at higher voltage than its “-” input. We should therefore expect that whenever voltage at the capacitor (“P2-Trigger”) falls down to 1/3V+ (i.e. 1/3rd of V+) the output of the comparator goes to V+ so right-click in the waveform window and “Add plot pane” then click on the 1/3V+ net and also on capacitor’s pin (“P2-Trigger”) and see: when the capacitor falls down to 1/3V+, comparator’s output goes high for a short moment and set SR Flip Flop’s output. The voltage at the capacitor starts rising again above 1/3V+ (we’ll see why): that’s why the comparator actually pulses rather than staying high.
  
  ![](/Images/7.jpg)
 
The voltage at the capacitor feeds also another comparator (U1 in the picture). This time it feeds the positive input and the negative input is at 2/3V+. This means that when voltage at the capacitor grows higher than 2/3V+, comparator output goes to V+, resetting the Flip Flop and making its output go to 0V.
Let’s check it: first make sure the focus is on the triangular waveform pane (click on it) then click on 2/3V+ network.
  
![](/Images/8.jpg)
  
Right click on the triangular waveform pane and select “Add plot pane” then click on the “Reset” net.
  
![](/Images/9.jpg)
  
The Reset input of the Flip Flop goes high to V+ when the voltage at the capacitor ramps to 2/3V+
The Set input of the Flip Flop goes low to 0V when voltage at the capacitors falls to 1/3V+.

The output of the 555 goes to V+ when the capacitor is charging, the output goes to 0V when the capacitor is discharging.

But what makes the capacitor discharge down to 1/3V+ and charge up to 2/3V+ ?

First: what makes it charge up? An easy one: it is the series of R5 and R6 that makes current flow and charge the capacitor. This is true when node MP is free or “floating” and this is the case when the collector of the NPN transistor is floating also, i.e. the transistor is not conducting, that is when “P3-Output” is to V+ (because the collector is to 0V only if “P3-Output is to 0V also, otherwise it is floating).

Second: what makes it discharge? Create a new pane (right click “Add new pane”) and click on node MP : when “P7-Disch” is low the capacitor discharges through R5 (the falling slope of the triangular waveform), when “P7-Disch” is high (floating) the capacitor charges through R5 and R6 (the rising slope of the triangular waveform).
  
![](/Images/10.jpg)
  
Let’s recap it:

The capacitor charges up to 2/3V+ through R5 and R6, then the SR Flip Flop is reset and P6-Output is reset to 0V. At that same time the transistor starts discharging the capacitor through R5 down to 1/3V+. When that lower threshold is reached the Flip Flop is Set to 1 and P3-Output goes to V+ and the transistor stops conducting and MP node goes floating again, current restart flowing through R5 and R6 and the capacitor restart charging again. And the loop is closed

That’s it.

A few things to note :
1) Voltage at the capacitor swings between 1/3V+ and 2/3V+ which are the values at the positive and negative inputs of the ineveters (see main schematic at top).
2) The smaller the resistors, the steepest the rise/fall time of the triangular waveform. Try it.
3) R6 contributes to rise time only, R5 contributes to both rise AND fall time of the triangular waveform.
4) The triangular waveform is not a triangular waveform, it has a logarithm and exponential shape.

Why it is not triangular (with linear slopes) instead of a logarithm ?

Voltage at a capacitor armature is given by V=Q/C where C is the capacitance (in Farad) and Q is the charge at its armatures, this is the definition of capacitance, V will the be given in Volt. Given C a constant, if Q increases over time with a given rule, that same rule is followed by V, that is, V rises linearly if and only if Q rises linearly and this happens only if Q (=I*t, this is the definition of current where I is the current, t is time) rises linearly with time also and this happens only if I is constant.

But this is not our case :  current I is not constant because as the capacitor charges, the voltage across the two resistors R5 and R6 drops, i.e. I = (V+ – Vcap)/(R5+R6) is not a constant. Voltage increase at the capacitor causes charging current drop so as that voltage growth over time is a function of voltage itself: definitely not linear a behavior. This can be demonstrated mathematically through a (say simple) differential equation.

Next step : make a sawtooth generator with linear ramp and steep fall.
  
