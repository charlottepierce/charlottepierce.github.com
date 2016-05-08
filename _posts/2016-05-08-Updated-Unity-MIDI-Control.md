---
layout: post
title: Updated Unity MIDI control
summary: A Unity interface allowing MIDI instrument inputs to trigger keyboard button presses.
---

_More information and the project code can be found on [Github](https://github.com/charlottepierce/UnityMidiControl) or [BitBucket](https://bitbucket.org/charlottepierce/unitymidicontrol)._

__UPDATED:__ This plugin now uses [keijiro's MIDI Jack](https://github.com/keijiro/MidiJack) as a backend, removing the need to run the third-party software MIDI Bridge required by the previous version.

This Unity interface allows you to map MIDI inputs to keyboard buttons - play the mapped input on your instrument to trigger the corresponding key press.
The interface currently supports `GetKey`, `GetKeyDown` and `GetKeyUp` events for all keyboard buttons.
Direct button presses (i.e., using the keyboard rather than a mapped instrument) will still be detected.

# Example Use #

The following note mappings trigger:

* the 'x' key when note number 36 is played on any MIDI channel
* the 'd' key when note number 50 is played on MIDI channel 8
* the 'a' key when a control knob on channel 22 has a value between 3 (exclusive) and 75 (inclusive) on any MIDI channel
* the 'b' key when a control knob on channel 31 has a value between 50 (exclusive) and 100 (inclusive) on MIDI channel 3

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