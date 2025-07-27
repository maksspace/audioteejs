# AudioTeeJS

AudioTeeJS wraps the [AudioTee](https://github.com/makeusabrew/audiotee) macOS swift binary and exposes it as a NodeJS package:

```ts
import { AudioTee } from 'audiotee'

const audiotee = new AudioTee({ sampleRate: 16000 })

audiotee.on('data', ({ data }) => {
  // handle raw PCM data
})

audiotee.start()

// ... later

audiotee.stop()
```
