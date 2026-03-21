![Whisper](./assets/banner.jpg)

<p align="center"><b>Robust Speech Recognition via Large-Scale Weak Supervision</b></p>

<p align="center">
  <a href="https://github.com/openai/whisper/blob/main/LICENSE">
    <img alt="GitHub license" src="https://img.shields.io/github/license/openai/whisper.svg">
  </a>
  <a href="https://github.com/openai/whisper/releases">
    <img alt="GitHub release" src="https://img.shields.io/github/release/openai/whisper.svg">
  </a>
  <a href="https://github.com/openai/whisper/graphs/contributors">
    <img alt="Contributors" src="https://img.shields.io/github/contributors/openai/whisper.svg">
  </a>
  <a href="https://github.com/openai/whisper/stargazers">
    <img alt="Stars" src="https://img.shields.io/github/stars/openai/whisper.svg?style=social">
  </a>
  <a href="https://github.com/openai/whisper/network/members">
    <img alt="Forks" src="https://img.shields.io/github/forks/openai/whisper.svg?style=social">
  </a>
</p>

<br>

<p align="center">
  <a href="#technical-details">Technical Details</a> •
  <a href="#fine-tuning">Fine-tuning</a> •
  <a href="#reference">Reference</a> •
  <a href="#license">License</a>
</p>

<br>

Whisper is a pre-trained model for automatic speech recognition (ASR) and speech translation, trained on ~680K hours of multilingual and multitask supervised data collected from the web. For more detail on its training, see the [paper](https://arxiv.org/abs/2212.04356).

This repository contains the code for running the Whisper model, including inference and fine-tuning, as well as the model weights.

## Install

Whisper can be installed via pip:

```bash
pip install -U openai-whisper
```

Alternatively, to install the latest version from the source code:

```bash
pip git+https://github.com/openai/whisper.git
```

On Ubuntu/WSL, you may need to install the following dependencies before installing whisper:

```bash
sudo apt update && sudo apt install ffmpeg
```

To enable GPU acceleration, install the CUDA-enabled version of PyTorch before installing whisper:

```bash
pip install torch --extra-index-url https://download.pytorch.org/whl/cu118
```

## Quick Start

Transcribe audio file(s) with the command line interface:

```bash
whisper audio.mp3 --language English --model medium
```

For the available models, see the [model sizes table](#model-details).

The command line interface is also available as a python module:

```python
import whisper

model = whisper.load_model("base")
result = model.transcribe("audio.mp3")
print(result["text"])
```

### In the browser

Whisper is available in [HuggingFace Transformers](https://huggingface.co/docs/transformers/main/en/model_doc/whisper), which runs entirely in the browser:

<p align="center">
  <a href="https://huggingface.co/spaces/openai/whisper">
    <img src="https://huggingface.co/datasets/huggingface/badges/resolve/main/open-in-hf-spaces-sm.svg" alt="HuggingFace Spaces">
  </a>
</p>

### Python package

The model can be loaded as a PyTorch module:

```python
import whisper

model = whisper.load_model("base")
```

The model can be specified with the `name` argument. Available options are: `tiny`, `base`, `small`, `medium`, `large`, `large-v2`, `large-v3`, `medium.en`, `small.en`, `tiny.en`, `base.en`, `small.en`.

When loading a model, the code will automatically download the model weights from HuggingFace.

The model accepts log-mel spectrograms as input. `whisper.audio` module provides helper functions to compute these:

```python
import whisper
import torch

device = "cuda" if torch.cuda.is_available() else "cpu"
model = whisper.load_model("large", device=device)

# load audio and pad/trim it to fit 30 seconds
audio = whisper.load_audio("audio.mp3")
audio = whisper.pad_or_trim(audio)

# make log-Mel spectrogram and move to the same device as the model
mel = whisper.log_mel_spectrogram(audio).to(model.device)

# detect the spoken language
_, probs = model.detect_language(mel)
print(f"Detected language: {max(probs, key=probs.get)}")

# decode the audio
options = whisper.DecodingOptions()
result = whisper.decode(model, mel, options)

print(result.text)
```

For inference on longer audio files, and for the advanced options (temperature, compression ratio, etc.), see the [API in the notebook](https://github.com/openai/whisper/blob/main/notebooks/LibriSpeech.ipynb).

## Model Details

| Model | English-only | Multilingual | Params | VRAM | Relative<br>speed |
| ----- | :----------: | :----------: | :----: | :--: | :---------------: |
| tiny  |     ✅       |      ✅      |  39 M  | ~1 GB |       ~32x        |
| base  |     ✅       |      ✅      |  74 M  | ~1 GB |       ~16x        |
| small |     ✅       |      ✅      | 244 M  | ~2 GB |       ~6x         |
| medium|     ✅       |      ✅      | 769 M  | ~5 GB |       ~2x         |
| large |             |      ✅      | 1550 M | ~10 GB|        1x         |
| large-v2 |        |      ✅      | 1550 M | ~10 GB|        1x         |
| large-v3 |        |      ✅      | 1550 M | ~10 GB|        1x         |

See [benchmark](https://github.com/openai/whisper/blob/main/notebooks/Benchmark.ipynb) for more details.

## Technical Details

Whisper is an encoder-decoder transformer (see the [Attention Is All You Need](https://arxiv.org/abs/1706.03762) paper). Input audio is re-sampled to 16kHz and processed as 80-channel log-mel spectrograms with a window size of 25 ms, a stride of 10 ms, and a maximum input length of 30 seconds. The encoder encodes the mel-spectrogram into a sequence of encoder hidden states. The decoder auto-regressively predicts the text tokens, conditioned on the encoder hidden states and the previously predicted tokens. The decoder also predicts special tokens that specify the spoken language (51 languages), task (transcription or translation), and timestamp (start and end time of each segment).

The model is trained on a diverse dataset of audio data, but the dataset is not explicitly labeled for specific dialects and accents. The model's ability to recognize accented speech may vary, and additional fine-tuning on specific dialects may improve performance. However, we have observed that the model generalizes well across dialects in the languages that it supports.

For the list of supported languages, see the [tokenizer details](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py).

### Available languages

The model supports transcribing in [these languages](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py#L10-L74), and translating from any of them to English.

The model is pre-trained on multilingual ASR dataset. See the paper for more detail on the dataset.

## Fine-tuning

Whisper has great performance on diverse speech recognition data, but it is still possible to fine-tune the model for a specific use case to improve performance.

We provide a fine-tuning notebook at `notebooks/Fine-tuning.ipynb`. For more details, see the [fine-tuning section](#fine-tuning) in this README.

### Fine-tuning Data

For fine-tuning, we recommend using a dataset of at least 100 hours of labeled audio. The data should be preprocessed to the following format:

- Audio files: single-channel wav files, 16kHz sampling rate.
- Transcripts: text files with the same name as the audio files, containing the transcription.

We provide a helper function `whisper.load_dataset` to load and preprocess common speech recognition datasets:

```python
import whisper
from datasets import load_dataset

# load the model first so the tokenizer is available
model = whisper.load_model("small", device="cpu")

# load a dataset from the hub
dataset = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

# load the audio and text
audio = dataset[0]["audio"]
text = dataset[0]["text"]

# tokenize the text
labels = model.tokenizer.encode(text)

# create the input features
input_features = whisper.log_mel_spectrogram(audio)

# fine-tune the model
# ...
```

For more details on fine-tuning, see the [fine-tuning notebook](notebooks/Fine-tuning.ipynb).

## Reference

If you use this model or the code in your paper, please cite:

```
@misc{radford2022whisper,
  title={Robust Speech Recognition via Large-Scale Weak Supervision},
  author={Alec Radford and Jong Wook Kim and Tao Xu and Greg Brockman and Christine McLeavey and Ilya Sutskever},
  year={2022},
  url={https://arxiv.org/abs/2212.04356}
}
```

## License

The code in this repository is licensed under the MIT license. The model weights are licensed under the [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) license, as described in the [Whisper License agreement](LICENSE).

## Related repos

1. related project [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper)
2. related project [m-bain/whisperX](https://github.com/m-bain/whisperX)