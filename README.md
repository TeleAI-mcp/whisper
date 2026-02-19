# Whisper

OpenAI's robust speech recognition system.

![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

## Installation

To install Whisper, run:

```bash
pip install -U openai-whisper
```

Alternatively, the following command will avoid pulling a large number of unnecessary dependencies:

```bash
pip install -U openai-whisper --no-deps
pip install -U tqdm
```

## Requirements

- Python 3.9+
- PyTorch
- ffmpeg

## Usage

The following command will transcribe audio from a file:

```bash
whisper audio.mp3
```

## License

MIT License

1. related project [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
2. related project [whisperX](https://github.com/m-bain/whisperX)