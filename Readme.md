# NE555 and LTSpice

Circuit simulation greatly simplifies, speeds-up and lowers cost of design of circuits. We might say that complex designs cannot be done without extensive simulation.
There are some tools for simulation and many of them are built around SPICE, some are also available at no cost. I’ve been using SPICE now and then with different names but the latest flavor for me is now Linear Technologies’ “LTSpice IV” :  it is freely downloadable from Linear Technologies’ website and works great.
It doesn’t have a very friendly schematic editor but The program  is great in all of the other juicy aspects: ease of use, libraries can be easily added with new components (though somewhat limited to Linear Tech’s components) and large has community support. Of course PSPICE models (that is, the description of the components necessary to simulate its behaviour) can be extended.
I’m not giving a tutorial here on LTSpice IV, there’s a lot around. Rather I’m using it to investigate how the now almost 40-years-old NE555 works. I’ll also show (on a next post) how a real NE555 can be made into a building block for a better sawtooth generator.

I did not make use of a ready-made NE555 model available, rather I designed the NE555 schematic from single macro-components to gain access to the electrical nodes inside the component.

You might just want to look at the pictures and follow the explanations or actually follow me step-by-step on your own schematic.

![](/Images/1.jpg)

