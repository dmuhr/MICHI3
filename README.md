# MICHI3 - Ableton live to grandMA3 OSC bridge

MICHI3 is a set of Max for Live objects that connect Ableton Live with grandMA3 lighting consoles. 

The group of objects are modular and can be used independently or through a server object, mainy for monitoring.

MICHI3 parses midi notes, midi CC and Automation curves to OSC messages that are understood as commands by the grandMA3 software, both on its ONPC or Console versions.
It provides an easy and intuitive way of using Live as a light sequencer together with MA3.

## How it works

MICHI3 grabs midi information from Ableton Live and parses it as commands to MA3. It can read MIDI notes, MIDI CC or velocity curves, and then map them to both fader and buttons functions of MA3.

MICHI3 contain the following functions:

Input types:
- MIDI Note
- MIDI CC
- Automation


## Setup

- Connect computer and console by ethernet. 
- Set your computer and console network interface to be in the same IP range. This can be a separate network interface or the one where you run the session.
- Disable firewall on the sending computer.

### grandMA3

- Start a session
  
Setup a new OSC receiver:
- Open grandMA3.
- Go to `Menu` -> `In & Out` -> `OSC`
- Select the `Interface` to be the physical input of the OSC. When running MICHI3 and ONPC in the same computer, use `127.0.0.1`
- Click on `Insert new OSCdata`
- Set the following:
  -  `Destination IP`: Empty
  -  `Mode`: UDP
  -  `Port`: 8880 (you can use any, but then also update on the MICHI3 object).
  -  `Prefix`: gMA3
  -  `Receive`: Yes
  -  `Send`: No
  -  `Receive Command`: Yes
  -  `Send Command`: No
  -  `Echo Input`: No (enable if you want to see the OSC input in the `System Monitor`, for debugging.
  -  `Echo Output`: No

### Ableton Live

- Save all MICHI3 objects in the same folder, you can also store then in your `Max MIDI Effect` folder, normally in: `/Users/[yourname]/Music/Ableton/User Library/Presets/MIDI Effects/Max MIDI Effect`
  - Important: You only need the objects ending in `.amxd`. The rest are dependencies, but they all must be in the same folder.
- Drag/drop MICHI3_Server from the `Browser content` of Ableton, or directly from your Finder window, and set:
  - `Console IP`: The IP of your console or ONPC computer. If the same computer use 127.0.0.1
  - `Port`: 8880 (or the port you just set in the MA3 OSC settings)

## Quick start

After you follow the setup instructions.

In grandMA3:
- Store a sequence, and give it a name.

in Ableton:
- Drag MICHI3_Single to a midi track.
- Write the same name you gave to the sequence before.
- Draw a midi note with a velocity higher than 0.

When the note is on in Ableton, in MA3 the button will be pressed with the function `Temp` and the fader will get the velocity of the midi note, scaled to `faderMaster`.

## Objects 

MICHI3 contains 4 different objects that can be used independently, and as many times as you wish inside your Ableton session (except MICHI3 Server). 

### MICHI3_Server

*Must be used only once!*

### MICHI3_Single

### MICHI3_Multi

### MICHI3_CueLoader




## Known limitations

- MICHI3 CueLoader: The scenes need to be set by automation before the note hits, and not at the same time. This is a limitation of Ableton, that gives less priority to the procesing of automation information. My solution: Just set `Scene` `Cue` or `Subscene` a bar before hitting the go with a MIDI note.
- If for some reason you edit one of the Max for Live patches, and you already have saved information in multiple MICHI3 objects, there is the chance that all that info will dissapear, and the objects will become completely empty (not even the default functions will show up). This is a bug of Ableton that hopfully will get fixed in Ableton 12.
  - My solution: Once in a while, I save all my MICHI3 object settings as Ableton `Preset`. Then if they all get corrupt, I just recall the presets by name.    This problem only happend 3 times to me in 2 years, but once it was a disaster. Always save versions :)




It uses all MA3 functions on sequences, independently if they are mapped or not to an executor. For instance, you can control speed and rate of a sequence, even though you only see the fadeMaster mapped to a knob. 

MICHI3 can also send fade times, which are sent directly as commands. They temporarily overwrite the written fade time of the cue. 

- All button and fader commands
- Fade in / out
- 

- 

## OBJECTS

MICHI3 contains 4 different objects that can be used independently, and as many times as you wish inside your Ableton session (except MICHI3 Server). 

MICHI3 SINGLE

Reads midi notes, CC or automation, and maps it to a Fader or Button of a MA3 sequence, called by name.

Other functionalities:
- Call an executor by number, instead of a sequence

MICHI3 MULTI

8x MICHI3 Single. For tracks when multiple notes need to be assigned to different MA3 sequences.

MICHI3 CUELOADER
Specifically made for working with long cuelists. They can be called by letters or numbers through automation.
MICHI3 SERVERReceives the data from all other objects, streams to the console and shows a monitor list.
