##MICHI3
## MICHI3

**ABLETON LIVE TO GRANDMA3 BRIDGE**

MICHI3 is a set of Max for Live objects that connect Ableton Live with grandMA3 lighting consoles. 

The group of objects are modular and can be used independently or through a “server” object, mainy for monitoring.

MICHI3 parses midi notes, midi CC and Automation curves to OSC messages that are understood as commands by the grandMA3 software, both on its ONPC or Console versions.
It provides an easy and intuitive way of using Live as a light sequencer together with MA3.


Objects:


MICHI3 SINGLE
Reads midi notes, CC or automation, and maps it to a Fader or Button of a MA3 sequence, called by name.




MICHI3 MULTI
8x MICHI3 Single. For tracks when multiple notes need to be assigned to different MA3 sequences.
MICHI3 CUELOADER
Specifically made for working with long cuelists. They can be called by letters or numbers through automation.
MICHI3 SERVERReceives the data from all other objects, streams to the console and shows a monitor list.
