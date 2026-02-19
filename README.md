# Whisper

Whisper is a general-purpose speech recognition model. It is trained on a large dataset of diverse audio and is also a multi-task model that can perform multilingual speech recognition as well as speech translation and language identification.

## Approach

![Approach](https://github.com/openai/whisper/raw/main/approach.png)

A Transformer sequence-to-sequence model is trained on various speech processing tasks, including multilingual speech recognition, speech translation, spoken language identification, and voice activity detection. All of these tasks are jointly represented as a sequence of tokens to be predicted by the decoder, allowing a single model to replace many different stages of a traditional speech processing pipeline. The multitask training format uses a set of special tokens that serve as task specifiers or classification targets.

## Setup

You need Python 3.9+ and PyTorch 1.10.1 to use this code. You also need to install the OpenAI Whisper model:

```bash
pip install -U openai-whisper
```

Alternatively, the following command will allow you to upgrade Whisper to its latest version (also ensuring you have the required dependencies):

```bash
pip install --upgrade --force-reinstall "openai-whisper @ https://github.com/openai/whisper/archive/main.tar.gz"
```

## Usage

The following command will transcribe speech in audio files:

```bash
whisper audio.flac audio.mp3 audio.wav --model medium
```

The default setting (which selects the `small` model) works well for transcribing English. You can increase the transcription accuracy by using a larger model, but this will be slower. For more information, run:

```bash
whisper --help
```

## Web Demo

[![Hugging Face Spaces](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue)](https://huggingface.co/spaces/openai/whisper)

## Related Projects

1. related project [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
2. related project [whisperX](https://github.com/m-bain/whisperX)