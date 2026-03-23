# Edge TTS (Microsoft, free)

[Edge TTS](https://github.com/rany2/edge-tts) uses Microsoft’s **free** text-to-speech API (the same as the “Read aloud” feature in the Edge browser). No Azure account required. **70+ languages** and **300+ voices**.

## Installation

```bash
pip install edge-tts
```

For WAV output, **pydub** is also required (for MP3→WAV conversion); it is usually already installed for Pocket TTS:

```bash
pip install pydub
```

## Enable Edge TTS

In **`config.json`**:

```json
{
  "tts": {
    "edge_tts_enabled": true
  }
}
```

Restart the server. On startup, all available Edge voices are fetched and appear in the voice list (e.g. `de-DE-KatjaNeural`, `en-US-AriaNeural`).

## Skip the Pocket TTS model (faster startup, less RAM/VRAM)

If you only use **Edge TTS** (and optionally **Piper**), you can avoid loading the large **Pocket TTS** neural model at startup:

```json
{
  "tts": {
    "edge_tts_enabled": true,
    "pocket_tts_model_enabled": false
  }
}
```

- **Pocket** clone voices will not work until you set `pocket_tts_model_enabled` back to `true` and restart.
- **Edge** and **Piper** are unchanged.
- `GET /health` reports `pocket_tts_model_loaded` separately from `tts_available` (any backend).
- With `pocket_tts_model_enabled: false`, the **`pocket_tts` Python package is not imported** at startup, so **torch / XPU / CUDA are not pulled in** for Pocket TTS (that was the main source of “VRAM used anyway”).
- The **`piper` package is also imported only when a Piper voice is actually used** (not at server startup), so **onnxruntime** does not touch the GPU until then — relevant if you had Piper installed but only use Edge voices.

## Voices

- **Voice ID** matches the Microsoft ShortName, e.g. `de-DE-KatjaNeural`, `en-US-GuyNeural`.
- All voices appear in `/v1/audio/voices`, Voice Chat, and **Wyoming** (Home Assistant).
- For Wyoming, language is derived from the voice locale (e.g. `de-DE` → German), so when HA language is set to German, the matching Edge voices are offered.

## Usage

- **API:** `POST /v1/audio/speech` with `"voice": "de-DE-KatjaNeural"` (or any other Edge voice ID).
- **Wyoming / Home Assistant:** Edge TTS appears alongside Pocket and Piper voices; language is mapped automatically.
- **Voice Chat:** Edge voices can be selected like any other voice.

## Notes

- Synthesis uses the Microsoft Edge TTS API (internet connection required).
- Normal Microsoft terms of use apply; usage is equivalent to the browser “Read aloud” feature.
- No API keys or sign-in required.

## References

- [edge-tts on GitHub](https://github.com/rany2/edge-tts)
- [edge-tts on PyPI](https://pypi.org/project/edge-tts/)
- List voices: `edge-tts --list-voices`
