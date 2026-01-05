# CapsLock Toggle Functionality Discrepency Fix

Also known as the Linux Caps Lock Delay.
Between Linux and other OSes such as Windows and OS X, there is a slight discrepency on how the Caps Lock toggle deactivation works. The discrepency is between how the toggle deactivates on the second press of the CapsLock key. On Windows (and OS X) it deactivates on clicking CapsLock key, while on Linux it deactivates upon releasing the key instead. Refer to the event table below for a detailed example of the discrepency.


|Action|Windows Output|Linux Output|
|---|--|---|
|Click & Release `a`| a | a|
|Click `CapsLock` | a | a |
|Click & Release `b` | aB | aB|
|Release `CapsLock` | aB | aB|
|Click & Release `c` | aBC | aBC |
|Click `CapsLock` | aBC | aBC |
|Click & Release `d` | aBCd | aBCD|
|Release `CapsLock` | aBCd | aBCD|
|Click & Release `e` | aBCde | aBCDe |

Caveat: This might not be true in all Linux distros. My experience has been with PopOS.

Needless to say, if you're coming from Windows this might be an annoying change. Without getting into the arguments of why anyone would use CapsLock or how this is the linux way of doing things, let me provide you my solution.

## The Solution

1. Naviate to the directory `/usr/shared/X11/xkb/symbols`.

In this directory you will find the language configurations to many different keyboard layouts. There should be a file called `pc`. Many of the other keyboard layouts have a dependency on this file, including the US keyboard.

2. Open the file `pc` and replace `key <CAPS> { [ Caps_Lock ] };` with `key <CAPS> { repeat=no, type[group1]="ALPHABETIC", symbols[group1]=[ Caps_Lock, Caps_Lock], actions[group1]=[ LockMods(modifiers=Lock), Private(type=3,data[0]=1,data[1]=3,data[2]=3)] };`. Save (sudo may be required).

This will replace the existing Caps_Lock functionality, with the one in Windows.

3. Restart your computer.

From my experience, this persists through restart unlike other solutions and works prior to logging in as well.

https://github.com/hyprwm/Hyprland/discussions/11227
