# Face-Tracking Desk Fan

A desk fan that finds a face in a camera frame and turns to follow it, on two servo axes.
Built over three generations — a thermostat, then ultrasonic tracking, then computer vision.

**Write-up:** https://jojcdm.github.io/face-tracking-fan/

## How it works

A webcam frame goes to MediaPipe's face detector. The face's offset from the centre of the
frame becomes an error in X and Y, which maps onto a pan angle and a tilt angle. Those two
numbers travel to an Arduino Uno over USB at 115200 baud as one line — `P95 T110 F1` — and
the firmware eases each servo toward the target a couple of degrees per tick, with a
dead-band near centre so it stops hunting. The same line switches the fan through a
logic-level MOSFET, with a 1.5 s linger after the face leaves and a 2 s link timeout that
shuts the fan off if the script on the laptop dies.

## Hardware

| Part | Notes |
| --- | --- |
| Arduino Uno | Signal only, powered over USB — never carries motor current |
| 2 × MG996R servos | Pan on D9, tilt on D10, metal gear, on a 6 V rail |
| Arctic P12 Pro LN | 120 mm, 12 V, 0.11 A |
| Logic-level MOSFET | Low-side switch on D7, 220 Ω gate series, 10 kΩ pull-down |
| 12 V wall adapter | Feeds the fan directly and a 12 → 6 V buck for the servos |
| 3 printed parts | Base, lid and fan cradle, modelled parametrically in OpenSCAD |

Single common ground. Splitting the supplies this way is what fixed the brownout resets —
the write-up covers that failure and three others.

## Next

Moving the vision onto a Raspberry Pi with the camera on the fan head, which closes the
loop properly: the fan turns until the face is centred in its *own* view rather than
mapping a fixed webcam's view onto an angle.

---

Jack Johnston · jjackjohnston05@gmail.com
