# AudioTee.js

AudioTee.js exposes the system audio capture functionality of the underlying [AudioTee](https://github.com/makeusabrew/audiotee) macOS swift binary as a Node.js package:

```ts
import { AudioTee } from 'audiotee'

const audiotee = new AudioTee({ sampleRate: 16000 })

audiotee.on('data', ({ data }) => {
  // data contains a raw PCM chunk of captured system audio
})

await audiotee.start()
// ... later
await audiotee.stop()
```

## Installation

`npm install audiotee`

Installation will download a prebuilt universal macOS binary which will run on Apple and Intel chips.

## Requirements

- macOS >= 14.2

## API stability

Until a >= 1.x.x release, the API is unstable and subject to change without notice.

## License

The MIT license.
