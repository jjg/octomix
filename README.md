# octomix

A means of simutaniously recording multiple musicians across the Internet.

## Example

In this example *engineer* refers to the person performing the recording, and *musician* refers to the people being recorded.  In this example everyone is in a separate physical location using a device connected to the Internet.

The engineer creates a new session, adds a track for each musician and asks the musicians to join.  As each musician joins, their associated track indicates to the engineer that they are ready.  When everyone is ready to record, the engineer presses "record" and each musician hears a click track to play along to (the musicians hear only the click, not the other musicians).  As the musicians play, the engineer monitors a mix of all musicians playing together.  When the musicians are done playing, the engineer presses "stop" and the mix can be played-back for all to hear.


## How it works

AFAIK it's impossible for musicians to play in sync together over the Internet due to the inherent delay in both analog-to-digital conversion (ADC) and network latency.  ADC latency can be reduced enough to avoid this but the nature of the Internet makes it virtually impossible to elimiate these delays.

When the engineer presses "record", a signal is sent to each musicians device telling it to start playing a click track.  This track is generated *locally*, on the musicians device so there is no percevable delay between when the click is generated and the musician hears it.  When the musician plays to the click, their playing is also recorded locally.  This minimizes any impact transmitting the data might have on the recording (delay, drop-outs, etc.) and allows the musical signal to be captured at the highest possible quality.  In addition to the music, this recording includes a timecode syncronized with the click track, so that the click and the audio are perfectly in sync (or at least as perfectly as the musician is capable of playing).

As this signal is recorded locally, it may also be streamed back to the engineer's machine where it is mixed with streams from the other musicians in the session.  All streams are re-syncronized using each musicians click track timecode, so any delay in transmission can be corrected before the audio is delivered to the engineer.  As a result the audio monitored by the engineer will be played "behind" the musicians in reality, but from the engineer's perspective all tracks will sound perfectly in-sync.

Under normal conditions, the engineer's monitor signal should only be delayed by a few seconds.  While it's possible for the system to function under more extreme latency (technically each musician could play hours apart) this may detract from the utility of recording simutaniously and as such parameters of the monitor stream can be adjusted to compensate for connection quality.


## Implementation

The system is composed of two devices: the engineer's console and the musicians interface.

The engineer's console is a piece of software that can run on a dedicated workstation or a regular desktop computer.  I'm not sure at the moment what it's going to be written in but for the purpose of the proof-of-concept it will probably be Python.

The musician's interface is a custom piece of hardware that contains an off-the-shelf single-board-computer (SBC), a signal generator (to produce the audible click), potentially a high-quality ADC/codec (if not provided by the SBC) and the requisite physical interface hardware (signal jacks, preamplifiers, mixers, knobs, etc.).  

The musician's interface communicates with the engineer's console via standard Internet protocols.  Ideally this connection is direct, but may require a public server component in order to facility NAT/firewall traversal.



## References

* http://iot-bits.com/interfacing-an-audio-codec-with-esp32/
