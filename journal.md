# Journal

## 03222020

Initial entry.

Create this repository a few days ago to start centralizing any notes, files, code, etc. related to the project.  

Today I'm adding some notes from Derek in the [docs/dereks_notes](./docs/dereks/notes) directory.  They provide a lot of detail about the actual recording process and will really inform what is and what isn't useful for us to build.  Hopefully there won't be anything essential that we can't figure out how to do.

I haven't yet disqualified making this whole thing run in a browser, and the more we look at how it would actually be used the more compelling that implementation becomes.  Even if we can't do *everything* without some native code (or even custom hardware) there's parts of this that will likely be browser-based regardless.

There's a lot of potential functionality here, but there's plenty of unknowns as well, and we should resolve those before we sink too much time into potential dead-ends.  So what does a proof-of-concept look like that is capable of vetting the implementation?

Three people, three computers.  One person (the engineer) clicks "record", the other two hear a click track.  The other two (musicians) start playing and their music is recorded locally.  The local recordings are returned to the engineers computer and played back in sync.

If we go the web app route, we have two pages: one for the engineer and one for the musicians.  The engineer page displays links that can be used by musicians to load the musician page and attach to the recording "session".  Once connected, the engineer page can start and stop a click track on the musician pages.

This isn't a bad place to start, and I think [WebRTC](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) is the first place to look.

After spending about 45 minutes getting up-to-date on what can be done in a browser, I'm reminded of why I stopped writing web software.  Perhaps I just need to take smaller bites, but it's such a mess trying to do the simplest things, and right now I'm leaning toward building custom hardware for this thing.
