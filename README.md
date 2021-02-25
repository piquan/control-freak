# Control Freak

Control Freak is a utility to control other programs with your
joystick or other input devices.

This version is demo code that the authors passed around privately to
discuss basic design and implementation.  It is released because it
may be useful to some people in this very simple, but working, form.

Here’s a quick discussion of the implementation of this version:

Everything is hardcoded here, but that means there aren't a lot of
hidden moving parts.  On my joystick, it will send a Shift-V when you
press the joystick's A button.

The dispatch table is the “chained if” (which only has one condition
right now) in `handle_js`.

To ensure that chords are handled properly, it will send everything
but the last button, then 10ms later send the last button, then 10ms
after that release them all.  (It's actually telling the X server to
queue all those key events with those times, but sends the event
stream all at the same time.)

This uses the XTEST extension to send these as “global” keyboard
events, rather than sending synthetic events to a particular window.
See “SENDEVENT NOTES” in the
[xdotool](https://github.com/jordansissel/xdotool) manpage.  If you
want it to send events to a particular window, you’ll need to change
some stuff.

This version doesn’t react well to things like a disconnected joystick.

If the KeyRelease events don’t get sent, then you might be stuck with
a modifier held down; in that case, you can just tap the keys on your
keyboard to make sure they’re seen as released.  (I have to do that a
lot when I alt-tab to and from a VNC session, since the alt down will
be seen by VNC but not the alt up.)
