# Whisper

OpenAI's robust speech recognition system.

## Installation

```bash
pip install openai-whisper
```

## Usage

```python
import whisper

model = whisper.load_model("base")
result = model.transcribe("audio.mp3")
print(result["text"])
```

## Available Models

| Size | Parameters | English-only model | Multilingual model | Required VRAM | Relative speed |
|------|------------|-------------------|-------------------|---------------|----------------|
| tiny | 39 M | tiny.en | tiny | ~1 GB | ~32x |
| base | 74 M | base.en | base | ~1 GB | ~16x |
| small | 244 M | small.en | small | ~2 GB | ~6x |
| medium | 769 M | medium.en | medium | ~5 GB | ~2x |
| large | 1550 M | N/A | large | ~10 GB | 1x |

## License

The model weights are released under the MIT License. See [LICENSE](LICENSE) for more information.

1. related project [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper)
2. related project [m-bain/whisperX](https://github.com/m-bain/whisperX)