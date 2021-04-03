# pulse-dialing

Arduino Uno Sketch to *simulate* a telephone exchange (Vermittlungsstelle) for a *single telephone* using
Pulse Dialing (US)/Loop-Disconnect Signalling (UK)/Decadic Signalling (AU)/Impuls Wahlverfahren (DE)
(e.g., a rotary dial telephone).

## Scope
This project is intended to breath just enough life into an old rotary dial
telephone to

1. make the phone ring, and
1. make it interesting for children to explore.

As written it uses the *German* standard for state machine, dial tones and signals, timing, 
and frequency of the bell (yes, phones used to ring differently in different countries).

## Limitations
* This program does not manage connections between multiple phones.
* Hook Flash is not implemented
* Sound quality is limited by hardware limitation of the Arduino Uno.

## Hardware
Intended to be used with Arduino Uno and Silvertel Ag1170-5 Low Power Ringing SLIC
- wire up Ag1170 as per datasheet
- connect `pin_FR` to `F/R`, 
  `pin_RM` to `RM`, and 
  `pin_SHK` to `SHK`
 - connect audio out from Arduino Uno as follows
   (turns square wave into something more sine-like and sets volume):

```
   ARDUINO                                ||          ||         Ag1170
   pin_AudioOut -----[100 kOhm]-----o-----||----------||-------- Vin
                                    |     ||          ||
                            100 nF ---      10 nF      100 nF
                                   ---
                                    |
                                   === GND
```

 - optional: monitor UART output for status information

I use a USB adapter to power the Arduino, and a separate adapter to power the Ag1170:
that little guy needs a lot of power to make the phone ring.
My Ag1170-5 sits on a breadboard without any protection diodes shown on the datasheet
(not a recommended practice) and it works just fine.

## Usage
Hook up the phone to the Ag1170 (a: brown, b: white) and

* pick up the handset to hear the dial tone (Wählton)
* dial `0`, hang up and wait for 3 s for the phone to start ringing (Rufsignal)
* dial `5` to hear "kein Anschluss unter dieser Nummer" (Hinweiston)
* dial `6` to hear either the ringing tone (Freiton) or the busy signal (Besetztton), selected randomly
* be inactive for too long and get kicked off the line (Gassenbesetztton) and eventually disconnected

Other multi-digit numbers are implemented, see code or UART output for details.
