---
layout: post
title: Unity MIDI control
summary: A Unity interface allowing MIDI instrument inputs to trigger keyboard button presses.
---

_More information and the project code can be found on [Github](https://github.com/charlottepierce/UnityMidiControl) or [BitBucket](https://bitbucket.org/charlottepierce/unitymidicontrol)._

This Unity interface is a project enable the mapping of MIDI inputs to keyboard buttons - play the mapped input on your instrument to trigger the corresponding key press. Any instrument detected by UnityMidiBridge ([Windows](https://github.com/keijiro/unity-midi-bridge/raw/master/midi-bridge-windows.zip); [OSX](https://github.com/keijiro/unity-midi-bridge/raw/master/midi-bridge-osx.zip)) should work.

# Example Use #

The following note mappings trigger:

* the 'x' key when note number 36 is played
* the 'd' key when note number 50 is played
* the 'a' key when a control knob on channel 22 has a value between 3 (exclusive) and 75 (inclusive)

![Example key mappings](https://bitbucket.org/charlottepierce/unitymidicontrol/raw/master/example_mappings.png)

These keypresses may be detected programmatically using the following code:

	if (UnityMidiControl.Input.InputManager.GetKeyDown("x")) {
		Debug.Log("'x' down");
	}
	if (UnityMidiControl.Input.InputManager.GetKeyDown("d")) {
		Debug.Log("'d' down");
	}
	if (UnityMidiControl.Input.InputManager.GetKeyDown("a")) {
		Debug.Log("'a' down");
	}
	
Using key codes rather than string arguments will also work:

	if (UnityMidiControl.Input.InputManager.GetKeyUp(KeyCode.X)) {
		Debug.Log("'x' up");
	}
	if (UnityMidiControl.Input.InputManager.GetKeyUp(KeyCode.D)) {
		Debug.Log("'d' up");
	}
	if (UnityMidiControl.Input.InputManager.GetKeyUp(KeyCode.A)) {
		Debug.Log("'a' up");
	}