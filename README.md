### MICHI3 ABLETON LIVE TO GRANDMA3 BRIDGE

MICHI3 is a set of Max for Live objects that connect Ableton Live with grandMA3 lighting consoles. 

The group of objects are modular and can be used independently or through a server object, mainy for monitoring.

MICHI3 parses midi notes, midi CC and Automation curves to OSC messages that are understood as commands by the grandMA3 software, both on its ONPC or Console versions.
It provides an easy and intuitive way of using Live as a light sequencer together with MA3.

## HOW IT WORKS

MICHI3 grabs midi information from Ableton Live and parses it as commands to MA3. It uses all MA3 functions on sequences, independently if they are mapped or not to an executor. For instance, you can control speed and rate of a sequence, even though you only see the fadeMaster mapped to a knob. 

MICHI3 can also send fade times, wich are sent directly as commands. They temporarily overwrite the written fade time of the cue. 

- 

## OBJECTS

MICHI3 contains 4 different objects that can be used independently, and as many times as you wish inside your Ableton session (except MICHI3 Server). 

# MICHI3 SINGLE

Reads midi notes, CC or automation, and maps it to a Fader or Button of a MA3 sequence, called by name.

Other functionalities:
- Call an executor by number, instead of a sequence

# MICHI3 MULTI

8x MICHI3 Single. For tracks when multiple notes need to be assigned to different MA3 sequences.

# MICHI3 CUELOADER
Specifically made for working with long cuelists. They can be called by letters or numbers through automation.
MICHI3 SERVERReceives the data from all other objects, streams to the console and shows a monitor list.
