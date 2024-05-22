# MICHI3 - Ableton live to grandMA3 OSC bridge

MICHI3 is a set of Max for Live objects that connect Ableton Live with grandMA3 lighting consoles. It provides an easy and intuitive way of using Live as a light sequencer together with MA3, taking full advantage of all automation and MIDI tools of Ableton Live. 

It is also a powerful tool to synchronize sound with precision, with a WAY nicer interface than timecode. The commands can also be stored as timecode later and it has been tested with Ableton Link and MIDI timecode.

MICHI3 parses midi notes, MIDI CC and Automation curves to OSC messages that are understood as commands by the grandMA3 software, both on its ONPC or Console versions. The group of objects is modular and can be used independently or through a server object, mainy for monitoring.

## Features

MICHI3 can:

- Map MA3 sequences by name or number.
- Read different inputs (MIDI Notes, MIDI CC, Automation curves).
- Set fade times up to 90 seconds.
- Press (and unpress) this button functions: `>>>` `<<<` `Black` `Flash` `Go+` `Go-` `LearnSpeed` `On` `Off` `Pause` `Rate1` `Select` `SelFix` `Speed1` `Swap` `Time` `Temp` `Toggle` `Top`.
- Set fader values on this functions: `Master` `X` `XA` `XB` `Temp` `Rate` `Speed` `Temp`.
- Set `AutoStart` and `AutoStop`.
- Call cues by name (or number) inside a cuelist
- Talk to different consoles/computers at the same time.
- Mute/solo MIDI tracks, independent of their output settings. 

## Objects 

MICHI3 contains 4 different objects that can be used independently, and as many times as you wish inside your Ableton session (except MICHI3 Server). 

### MICHI3_Server

*Must be used only once!*
- Receives all OSC data and streams it to a Console or ONPC.

### MICHI3_Single

- The most basic object.
- The only one that can read CC and automation curves.

### MICHI3_Multi

- 8x MICHI3_Single
- Reads only in single`NOTE` mode.

### MICHI3_CueLoader

- Specific for dealing with long cuelists.
- The only one that can send `Goto` / `load` commands. 
- Has separate MIDI assignments for cue, fader, button and `Off`. 
- Cues can be called by Number or Name, choose in the config tab opening with the little arrow. 
    - When calling by number, use the `CueNum` automation track. 
    - When calling by name, use the `CueScene` automation track, to call by Letter, i.e. "B".
    - Alternative: You can also form names with `CueScene` and `CueNum`, i.e. "B4".
    - Alternative 2: You can also form names with `CueScene`, `CueNum` and `CueSubNum`. They will be formated with a dash in between, i.e. "B4-3". 
    - *IMPORTANT!* When calling cues by Name, their names need to be exactly the same in the cuelist.

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


## Details

### Names

`Seq` / `Exec` 
- Alternate between using Sequence (either by number or name) or a executor. `Seq` is recommened. In this last case, the executor needs to be called by number (i.e. 101), and it will work on any page, as the usual MIDI input of grandMA2. The limitation (till v. 2.0.2.0) is that no fader information can be assigned in exec mode.

- Names in MA3 are unique. But MICHI3 doesn't know that, so be careful with the naming :) All names will be turned to ALL CAPS, but MA3 is not case sensitive for names. 

### Buttons and Faders

- Following the new sequence structure of MA3, all button and fader parameters can be applied to a seauence, even though they are not visibly assigned to an executor. (i.e., I can store a simple sequence on exec 101, and assign it temp. But with a couple of MICHI3_Single, I sned values for speed1, speed, rate, master and go).

`Fadein` `Fadeout` 
- They temporarily overwrite `CueInFade` of Cue 1 of a sequence, and `CueInFade` of `OffCue`. Usefull when using sequences as `temp` buttons.
- They can be freely automated, but set it a bit before the note comes. Ableton is slow when it comes to Automation and M4L.
Note: it only works good for `Temp` and `Go+` `Go-`. 

### Input Mode:

In all cases, you can select `All` / `Single`. In all, it will listen to whatever MIDI note present. In single, you have to choose. 

- `NOTE`: 
  - Velocity of the note = Fader position (Scaled 0 to 100%, input range adjustable)
  - Note on = button pressed, Note off = button unpressed. (Adjustable with Threshold)

- `CC`:
  - Select CC channel (20 by default) 
  - CC Value = Fader position (Scaled 0 to 100%, input range adjustable)
  - Note on = button pressed, Note off = button unpressed. (Adjustable with Threshold)
  - *IMPORTANT*: To avoid constant value output or mistakes, the CC curve is only read when a note is on. If no button command is needed, just deactivate the button.

- `AUTO`:
  - Automate the parameter called `Autofader`.
  - It works the same as CC. A MIDI note needs to be present to get the input. 

### Configurations

`Un`
- Unpress. Normally when we get a Note off, the button will unpress. This doesn't work for certain commands like `toggle` or `top`. In this cases, `Un` will be automatically deselected. The button can also be automated.

Priority: 
- By default, the Fader information goes out miliseconds before the button, so i.e. we can set the dimmer before we `Go+`; if `autoStart` is disabled that would be the only way to get output. 

`AutoStart` `AutoStop`
- Send this commands directly to `EditSetting`.

Destination
- Where is the output of the object going:
  - Server would mean it all goes to MICHI3_Server, which should be accesible in a MIDI track. 
  - Console means the OSC goes directly to the specified IP and port. 



## Known limitations

- MICHI3 CueLoader: The scenes need to be set by automation before the note hits, and not at the same time. This is a limitation of Ableton, that gives less priority to the procesing of automation information. My solution: Just set `Scene` `Cue` or `Subscene` a bar before hitting the go with a MIDI note.
- If for some reason you edit one of the Max for Live patches, and you already have saved information in multiple MICHI3 objects, there is the chance that all that info will dissapear, and the objects will become completely empty (not even the default functions will show up). This is a bug of Ableton that hopfully will get fixed in Ableton 12.
  - My solution: Once in a while, I save all my MICHI3 object settings as Ableton `Preset`. Then if they all get corrupt, I just recall the presets by name.    This problem only happend 3 times to me in 2 years, but once it was a disaster. Always save versions :)


## Buy me a coffee :)

https://buymeacoffee.com/diegomuhr
