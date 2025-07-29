# AudioTee.js

AudioTee.js captures your Mac's system audio output and emits it as PCM encoded chunks at regular intervals. It's a tiny Node.js wrapper around the underlying [AudioTee](https://github.com/makeusabrew/audiotee) swift binary, which is [bundled](./bin) in this repository and distributed with the package published to [npm](https://www.npmjs.com/package/audiotee).

## Basic usage

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

Unless otherwise specified, AudioTee will capture system audio from all running processes.

## Installation

`npm install audiotee`

Installation will download a prebuilt universal macOS binary which runs on both Apple and Intel chips, and weighs less than 600Kb.

## Options

The `AudioTee` constructor accepts an optional options object:

```ts
interface AudioTeeOptions {
  sampleRate?: number // Target sample rate (Hz), default: device default
  chunkDuration?: number // Duration of each audio chunk in seconds, default: 0.2
  mute?: boolean // Mute system audio whilst capturing, default: false
  includeProcesses?: number[] // Only capture audio from these process IDs
  excludeProcesses?: number[] // Exclude audio from these process IDs
}
```

### Option details

- **`sampleRate`**: Converts audio to the specified sample rate. Common values are `16000`, `44100`, `48000`.
- **`chunkDuration`**: Controls how frequently data events are emitted. Smaller values = more frequent events with smaller chunks. Specified in seconds
- **`mute`**: When `true`, system audio is muted whilst AudioTee is capturing
- **`includeProcesses`**: Array of process IDs to capture audio from (all others filtered out)
- **`excludeProcesses`**: Array of process IDs to **exclude** from capture

## Events

AudioTee uses an EventEmitter interface to stream audio data and system events:

```ts
// Audio data events
audiotee.on('data', (chunk: { data: Buffer }) => {
  // Raw PCM audio data - mono channel, 32-bit float or 16-bit int depending on conversion
})

// Lifecycle events
audiotee.on('start', () => {
  // Audio capture has started
})

audiotee.on('stop', () => {
  // Audio capture has stopped
})

// Error handling
audiotee.on('error', (error: Error) => {
  // Process errors, permission issues, etc.
})

// Logging
audiotee.on('log', (message: string, level: LogLevel) => {
  // System logs from the AudioTee binary
  // LogLevel: 'metadata' | 'stream_start' | 'stream_stop' | 'info' | 'error' | 'debug'
})
```

### Event details

Note: a bug in versions up to 0.0.2 means only the `data` lifecycle event is actually currently emitted.

- **`data`**: Emitted for each audio chunk. The `data` property contains raw PCM audio bytes
- **`start`**: Emitted when audio capture begins successfully
- **`stop`**: Emitted when audio capture ends
- **`error`**: Emitted for process errors, permission failures, or system issues
- **`log`**: Emitted for all system messages from the underlying AudioTee binary

## Requirements

- macOS >= 14.2

## API stability

During the `0.x.x` release, the API is unstable and subject to change without notice.

## Best practices

- Always specify a sample rate. Tell AudioTee what you want, rather than having to parse the
  `metadata` message to see what you got from the output device
- Specifying _any_ sample rate automatically switches encoding to use 16-bit signed integers, which is half the byte size compared to the 32-bit float the stream probably uses natively
- You'll probably need to specify a different `chunkDuration` depending on your use case. For example, some ASRs are quite particular about the exact length of each chunk they expect to process.

## License

### The MIT License

Copyright (C) 2025 Nick Payne.
