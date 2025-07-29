# AudioTee → AssemblyAI Universal Streaming Example

This example demonstrates how to stream macOS system audio captured by AudioTee directly to AssemblyAI's Universal Streaming API for real-time speech transcription.

## What this does

1. **Captures system audio** using AudioTee (whatever's playing on your Mac)
2. **Streams it in real-time** to AssemblyAI's Universal Streaming API
3. **Displays live transcriptions** with partial results updating in real-time
4. **Shows final formatted transcripts** when AssemblyAI detects end-of-turn

It is no more than a quick proof of concept. It is **not** production ready.

## Setup

1. **Install dependencies**:

   ```bash
   npm install
   ```

2. **Get your AssemblyAI API key**:

   - Sign up at [AssemblyAI](https://www.assemblyai.com/)
   - Get your API key from the dashboard

3. **Set your API key**:

   ```bash
   export ASSEMBLYAI_API_KEY="your-api-key-here"
   ```

## Usage

```bash
npm start
```

Or for development with auto-restart:

```bash
npm run dev
```

## What you'll see

```
Starting AudioTee -> AssemblyAI Universal Streaming example
Press Ctrl+C to stop

Connecting to AssemblyAI...
Starting system audio capture...

AssemblyAI session opened with ID: abc123...
AudioTee started capturing system audio
Listening for system audio...

hello this is a test of the system  # <- partial transcript (updates in real-time)
Hello, this is a test of the system.  # <- final formatted transcript
```

## Configuration

The example is configured with optimal settings for system audio transcription:

- **16kHz sample rate**: Balances quality and processing speed
- **50ms chunks**: AssemblyAI's recommended chunk size for minimal latency
- **Format turns enabled**: Gets punctuated, properly formatted final transcripts
- **PCM S16LE encoding**: 16-bit audio format

You can modify these settings in `index.ts`:

```typescript
const audioTee = new AudioTee({
  sampleRate: 16000, // Audio sample rate
  chunkDuration: 0.05, // 50ms chunks
})

const transcriber = client.streaming.transcriber({
  sampleRate: 16000,
  formatTurns: true, // Enable formatted final transcripts
  encoding: 'pcm_s16le',
})
```

## Requirements

- macOS 14.2+
- Node.js 18+
- AssemblyAI API key
- System audio recording permissions (prompted on first run)

## Troubleshooting

- **No audio detected**: Ensure something is playing audio on your Mac and that AudioTee has permission to access system audio
