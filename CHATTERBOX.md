# Chatterbox TTS support (Resemble AI)

[Chatterbox](https://github.com/resemble-ai/chatterbox) by Resemble AI provides **multilingual TTS** with **23+ languages** (e.g. German, French, Spanish, Chinese). You can use it alongside Pocket TTS and Piper.

## Setup

1. **Install Chatterbox (optional):**
   ```bash
   pip install chatterbox-tts
   ```
   Note: Chatterbox has its own dependencies (PyTorch, etc.). If you already use Pocket TTS, you may need to ensure compatible versions.

2. **Enable in config** – in `config.json` set:
   ```json
   {
     "tts": {
       "chatterbox_enabled": true,
       "chatterbox_model": "multilingual"
     },
     "paths": {
       "voices_chatterbox_dir": "voices-chatterbox"
     }
   }
   ```

3. **Restart the server.** The first time you use a Chatterbox voice, the model will be downloaded (lazy load).

## Voices

- **Language voices (no file needed):** The server adds one voice per language, e.g. `chatterbox-de`, `chatterbox-en`, `chatterbox-fr`. Use these for natural speech in that language. In Home Assistant (Wyoming), when you select **German**, only voices that support German (including `chatterbox-de`) are listed.
- **Reference-based cloning:** Put short WAV files (e.g. 10 s) in `voices-chatterbox/`. Each file becomes a voice (e.g. `my-speaker.wav` → voice `chatterbox-my-speaker`). Synthesis will clone that voice; you can still pass a language for multilingual output.

## Supported languages (Multilingual model)

Arabic (ar), Danish (da), German (de), Greek (el), English (en), Spanish (es), Finnish (fi), French (fr), Hebrew (he), Hindi (hi), Italian (it), Japanese (ja), Korean (ko), Malay (ms), Dutch (nl), Norwegian (no), Polish (pl), Portuguese (pt), Russian (ru), Swedish (sv), Swahili (sw), Turkish (tr), Chinese (zh).

## Config

| Key | Default | Description |
|-----|--------|-------------|
| `tts.chatterbox_enabled` | `false` | Turn Chatterbox on/off. |
| `tts.chatterbox_model` | `"multilingual"` | Currently only `multilingual` is supported. |
| `paths.voices_chatterbox_dir` | `voices-chatterbox` | Folder for reference WAVs (and subfolders are scanned). |

## Device

Chatterbox uses the same `tts.device` as Pocket TTS for **CUDA** or **CPU**. Intel Arc (XPU) is not supported by Chatterbox; it will fall back to CPU when XPU is set.

## API and Wyoming

- Chatterbox voices appear in `/v1/audio/voices` and work with `/v1/audio/speech`, Voice Chat (streaming and non-streaming), and **Wyoming** (Home Assistant).
- For Wyoming, each voice is reported with the correct language(s), so e.g. with HA language **German** you get `chatterbox-de` and other German-capable voices.

## References

- [Chatterbox on GitHub](https://github.com/resemble-ai/chatterbox)
- [Chatterbox Multilingual demo](https://huggingface.co/spaces/ResembleAI/Chatterbox-Multilingual-TTS)
