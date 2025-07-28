# AudioTee → AssemblyAI Universal Streaming Example

This example demonstrates how to stream macOS system audio captured by AudioTee directly to AssemblyAI's Universal Streaming API for real-time speech transcription.

## What this does

1. **Captures system audio** using AudioTee (whatever's playing on your Mac)
2. **Streams it in real-time** to AssemblyAI's Universal Streaming API
3. **Displays live transcriptions** with partial results updating in real-time
4. **Shows final formatted transcripts** when AssemblyAI detects end-of-turn

Perfect for transcribing video calls, meetings, podcasts, or any audio playing through your Mac's speakers.

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
   
   Or edit `index.ts` and replace `YOUR_API_KEY_HERE` with your actual key.

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

🔗 Connecting to AssemblyAI...
🎵 Starting system audio capture...

🎙️  AssemblyAI session opened with ID: abc123...
🎵 AudioTee started capturing system audio
Listening for system audio...

hello this is a test of the system  # <- partial transcript (updates in real-time)
📝 Hello, this is a test of the system.  # <- final formatted transcript
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
  sampleRate: 16000,     // Audio sample rate
  chunkDuration: 0.05,   // 50ms chunks
  mute: false            // Set to true to mute speakers while capturing
})

const transcriber = client.streaming.transcriber({
  sampleRate: 16000,
  formatTurns: true,     // Enable formatted final transcripts
  encoding: 'pcm_s16le'
})
```

## Requirements

- macOS 14.2+
- Node.js 18+
- AssemblyAI API key
- System audio recording permissions (prompted on first run)

## Tips

- **Audio permissions**: The first run will prompt for microphone permissions in System Preferences
- **Background audio**: Works great with any audio playing through your Mac - Zoom calls, YouTube videos, podcasts, etc.
- **Process filtering**: You can modify AudioTee options to capture only specific applications using `includeProcesses` or `excludeProcesses`
- **Muting**: Set `mute: true` in AudioTee options to silence your speakers while capturing (useful for avoiding feedback)

## Troubleshooting

- **"Unauthorized"**: Check your AssemblyAI API key
- **No audio detected**: Ensure something is playing audio on your Mac and that AudioTee has permission to access system audio
- **Connection errors**: Verify your internet connection and that AssemblyAI's service is accessible