# Chatterbox TTS support (Resemble AI)

[Chatterbox](https://github.com/resemble-ai/chatterbox) by Resemble AI provides **multilingual TTS** with **23+ languages** (e.g. German, French, Spanish, Chinese). You can use it alongside Pocket TTS and Piper.

## Compatibility (Python & NumPy)

The PyPI package **chatterbox-tts 0.1.6** pins **numpy<1.26**, while this project uses **numpy≥2.0**. On **Python 3.12**, building that old numpy from source also fails (`pkgutil.ImpImporter` was removed). So a plain `pip install chatterbox-tts` in the same venv as Pocket TTS often fails.

**Workarounds:**

- **Option A – Separate venv with Python 3.10 or 3.11 (recommended)**  
  Create a venv with Python 3.10 or 3.11, install this project’s dependencies and then `pip install chatterbox-tts`. That way Chatterbox gets numpy<1.26 and the rest of the stack stays consistent.

- **Option B – Install without Chatterbox deps (try at your own risk)**  
  In your existing venv (with numpy≥2.0):  
  `pip install chatterbox-tts --no-deps`  
  Then install Chatterbox’ other dependencies by hand (e.g. `torch`, `torchaudio`, `librosa`, `transformers`, etc. from [their pyproject](https://github.com/resemble-ai/chatterbox/blob/master/pyproject.toml)). Runtime may break if Chatterbox really needs numpy<1.26.

- **Option C**  
  Watch [chatterbox-tts on PyPI](https://pypi.org/project/chatterbox-tts/) or [GitHub](https://github.com/resemble-ai/chatterbox) for a release that supports numpy 2.x and Python 3.12.

## Setup

1. **Install Chatterbox (optional)** – see [Compatibility](#compatibility-python--numpy) above. If your environment is compatible:
   ```bash
   pip install chatterbox-tts
   ```

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
