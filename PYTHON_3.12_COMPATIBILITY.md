# Python 3.12 Compatibility Guide

## Overview

This Voice AI Training System has been updated to support **Python 3.12+** using compatible TTS (Text-to-Speech) libraries. The original Coqui TTS library does not support Python 3.12, so we've migrated to the Idiap Research Institute's fork which provides full Python 3.12 compatibility.

## What Changed?

### TTS Library Migration

**Previous (Python 3.9-3.11 only):**
```python
pip install TTS>=0.22.0
from TTS.api import TTS
```

**New (Python 3.12+ compatible):**
```python
pip install coqui-tts>=0.24.0
from TTS.api import TTS  # API remains the same!
```

### Key Points

1. **Package Name Changed**: `TTS` → `coqui-tts`
2. **API Unchanged**: The import and usage remain identical (`from TTS.api import TTS`)
3. **Full Compatibility**: Works with Python 3.9, 3.10, 3.11, and 3.12+
4. **Maintained Fork**: Actively maintained by Idiap Research Institute

## Installation

### Using requirements.txt

The updated `requirements.txt` now includes:

```txt
# Text-to-Speech Models (Python 3.12 Compatible)
coqui-tts>=0.24.0
```

Install with:
```bash
pip install -r requirements.txt
```

### Direct Installation

```bash
pip install coqui-tts
```

### Verify Installation

```python
import sys
import TTS

print(f"Python: {sys.version}")
print(f"TTS: Installed successfully")
```

## Alternative TTS Options

If you need different TTS solutions, here are Python 3.12 compatible alternatives:

### 1. Edge-TTS (Microsoft)
**Best for**: Cloud-based, lightweight, high quality

```bash
pip install edge-tts>=6.1.0
```

```python
import edge_tts
import asyncio

async def generate_speech():
    communicate = edge_tts.Communicate("Hello World", "en-US-AriaNeural")
    await communicate.save("output.mp3")

asyncio.run(generate_speech())
```

**Pros:**
- Excellent quality
- Multiple voices and languages
- Lightweight (no large models to download)

**Cons:**
- Requires internet connection
- API-based (rate limits may apply)

### 2. Bark (Suno AI)
**Best for**: Expressive speech, multiple languages

```bash
pip install git+https://github.com/suno-ai/bark.git
```

```python
from bark import SAMPLE_RATE, generate_audio, preload_models
from scipy.io.wavfile import write as write_wav

# Load models
preload_models()

# Generate audio
audio_array = generate_audio("Hello, my name is Bark.")
write_wav("bark_output.wav", SAMPLE_RATE, audio_array)
```

**Pros:**
- High quality, expressive speech
- Supports multiple languages
- Can generate non-speech sounds

**Cons:**
- Larger model size
- Slower inference
- May have limited Python 3.12 support (check compatibility)

### 3. Parler-TTS (Hugging Face)
**Best for**: Modern architecture, good quality

```bash
pip install parler-tts
```

```python
from parler_tts import ParlerTTSForConditionalGeneration
from transformers import AutoTokenizer

model = ParlerTTSForConditionalGeneration.from_pretrained("parler-tts/parler_tts_mini_v0.1")
tokenizer = AutoTokenizer.from_pretrained("parler-tts/parler_tts_mini_v0.1")

# Generate speech
inputs = tokenizer("Hello world", return_tensors="pt")
audio = model.generate(**inputs)
```

**Pros:**
- Modern transformer architecture
- Good quality
- Active development

**Cons:**
- Newer library (less mature)
- Fewer pre-trained models

### 4. pyttsx3 (Offline TTS)
**Best for**: Simple offline TTS, no GPU required

```bash
pip install pyttsx3
```

```python
import pyttsx3

engine = pyttsx3.init()
engine.say("Hello World")
engine.runAndWait()
```

**Pros:**
- Completely offline
- Very lightweight
- No GPU required

**Cons:**
- Lower quality (robotic voice)
- Limited customization

## Migration Checklist

If you're migrating existing code:

- [x] Update `requirements.txt`: Change `TTS>=0.22.0` to `coqui-tts>=0.24.0`
- [x] Reinstall dependencies: `pip install -r requirements.txt`
- [x] No code changes needed: The API is identical
- [x] Test your notebooks: Run setup.ipynb, train_or_finetune.ipynb, demo.ipynb
- [x] Verify Python version: `python --version` (should be 3.12+)

## Troubleshooting

### Issue: Import Error

**Error:**
```
ModuleNotFoundError: No module named 'TTS'
```

**Solution:**
```bash
pip uninstall TTS
pip install coqui-tts
```

### Issue: Version Conflict

**Error:**
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed.
```

**Solution:**
```bash
pip install --upgrade pip
pip install --upgrade coqui-tts
```

### Issue: CUDA/GPU Issues

**Error:**
```
RuntimeError: CUDA out of memory
```

**Solution:**
- Reduce batch size
- Use CPU mode: `TTS(..., gpu=False)`
- Clear GPU cache: `torch.cuda.empty_cache()`

## Compatibility Matrix

| Package | Python 3.9 | Python 3.10 | Python 3.11 | Python 3.12+ |
|---------|-----------|-------------|-------------|--------------|
| `TTS` (original) | ✅ | ✅ | ✅ | ❌ |
| `coqui-tts` (Idiap) | ✅ | ✅ | ✅ | ✅ |
| `edge-tts` | ✅ | ✅ | ✅ | ✅ |
| `bark` | ✅ | ✅ | ✅ | ⚠️ (limited) |
| `parler-tts` | ✅ | ✅ | ✅ | ✅ |
| `pyttsx3` | ✅ | ✅ | ✅ | ✅ |

## Additional Resources

- **Idiap Coqui TTS Fork**: https://github.com/idiap/coqui-ai-TTS
- **Original Coqui TTS**: https://github.com/coqui-ai/TTS (archived)
- **Edge-TTS**: https://github.com/rany2/edge-tts
- **Bark**: https://github.com/suno-ai/bark
- **Parler-TTS**: https://github.com/huggingface/parler-tts

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Review the Idiap Coqui TTS documentation
3. Open an issue on the respective GitHub repository

---

**Last Updated**: October 2025  
**Tested with**: Python 3.12.x, coqui-tts 0.24.x
