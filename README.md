# octomix

A means of simutaniously recording multiple musicians across the Internet.

## Example

In this example *engineer* refers to the person performing the recording, and *musician* refers to the people being recorded.  In this example everyone is in a separate physical location using a device running the software, connected to the Internet.

The engineer creates a new session, adds a track for each musician and asks the musicians to join.  As each musician joins their associated track indicates to the engineer that they are ready.  When everyone is ready to record, the engineer presses "record" and each musician hears a click track to play along to (the musicians hear only the click, not the other musicians).  As the musicians play, the engineer monitors a mix of all musicians playing together.  When the musicians are done playing, the engineer presses "stop" and the mix can be played-back for all to hear.


## How it works

AFAIK it's impossible for musicians to play in sync together over the Internet due to the inherent delay in both analog-to-digital conversion (ADC) and network latency.  ADC latency can be reduced enough to avoid this but the nature of the Internet makes it virtually impossible to elimiate these delays.

When the engineer presses "record", a signal is sent to each musicians device telling it to start playing a click track.  This track is generated "locally", on the musicians device so there is no percevable delay between when the click is generated and the musician hears it.  When the musician plays to the click, their playing is also recorded locally.  This minimizes any impact transmitting the data might have on the recording (delay, drop-outs, etc.) and allows the musical signal to be captured at the highest possible quality.  In addition to the music, this recording includes a timecode syncronized with the click track, so that the click and the audio are perfectly in sync (or at least as perfectly as the musician is capable of playing).  

As this signal is recorded locally, it is also streamed back to the engineer's machine where it is mixed with streams from the other musicians in the session.  All streams are re-syncronized using each musicians click track timecode, so any delay in transmission can be corrected before the audio is delivered to the engineer.  As a result the audio monitored by the engineer will be played "behind" the musicians in reality, but from the engineer's perspective all tracks will sound perfectly in-sync.

Under normal conditions, the engineer's monitor signal should only be delayed by a few seconds.  While it's possible for the system to function under more extreme latency (technically each musician could play hours apart) this may detract from the utility of recording simutaniously and as such parameters of the monitor stream can be adjusted to compensate for connection quality.


## Implementation

An ideal implementation would be purely software and work on the widest range of devices possible.  This raises a number of challenges vs. using specialized hardware running custom software, but the audience for a pure software solution is exponentially higher, so we'll start there.

The lowest-common-denominator is something that can run in a web browser.  Normally this would be absurd for anything involving precise timing, but since coping with latency and unreliability is what makes the design unique, this isn't an immediate showstopper.  The alternative is to produce (and maintain) a native application for each device which would probably be more work than producing specialized hardware. 

That being the case, here's what I'm thinking right now:

An HTML5 "web app" that consists of two primary interfaces: 

* the musician's recorder
* the engineers console

These two communicate primarilly via [websockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API), and will likely require a server to aid in firewall traversal (perhaps something [webrtc](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)).

Both interfaces will need to access device audio hardware and there is a webapi that might fit the bill: [WebAudio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API).

What's unclear to me at this point is whether or not these bits will be able to simutanously output the click track while recording the musician's audio.  If that's possible the next question is with regard to the best way to timestamp the recorded audio in sync with the click.  Right now I don't know enough about these API's to know, and since this is an unusual use case it may not be supported at all.  I imagine there's a way to play a sound and record at the same time, but this is the one stage in the system where timing is critical, so any syncronization error here undermines the entire system.  I know this level of accuracy is possible using more native programming interfaces, but if we can make it work in a browser, it's a huge win.


## References

* https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API
* https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
* https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API
* https://github.com/cwilso/metronome
* https://www.html5rocks.com/en/tutorials/getusermedia/intro/
* https://developers.google.com/web/updates/2012/09/Live-Web-Audio-Input-Enabled
