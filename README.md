# AudioTeeJS

AudioTeeJS exposes the macOS system audio capture functionality of the underlying [AudioTee](https://github.com/makeusabrew/audiotee) swift binary as a Node.js package:

```ts
import { AudioTee } from 'audiotee'

const audiotee = new AudioTee({ sampleRate: 16000 })

audiotee.on('data', ({ data }) => {
  // data contains a raw PCM chunk of captured system audio
})

audiotee.start()

// ... later

audiotee.stop()
```

## Installation

`npm install audiotee`

Installation will download a prebuilt universal macOS binary which will run on Apple and Intel chips.

## Requirements

- macOS >= 14.2
