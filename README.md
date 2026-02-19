# Whisper

Whisper is a general-purpose speech recognition model. It is trained on a large dataset of diverse audio and is also a multi-task model that can perform multilingual speech recognition, speech translation, and language identification.

## Approach

A Transformer sequence-to-sequence model is trained on various speech processing tasks, including multilingual speech recognition, speech translation, spoken language identification, and voice activity detection. All tasks are jointly represented as a sequence of tokens to be predicted by the decoder, allowing a single model to replace many different stages of a traditional speech processing pipeline. The multitask training format uses a set of special tokens that serve as task specifiers or classification targets.

## Model

Checkpoints are available at https://github.com/openai/whisper/blob/main/README.md#model-checkpoints

## Results

As shown in the table below, Whisper approaches human level robustness and accuracy on English speech recognition. 

## Installation

To install the package, run:

```
pip install openai-whisper
```

Alternatively, you can install the latest development version from GitHub:

```
pip install git+https://github.com/openai/whisper.git
```

## Usage

The codebase also provides a way to run inference on the model:

```
import whisper

model = whisper.load_model("base")
result = model.transcribe("audio.mp3")
print(result["text"])
```

## License

Whisper's code is licensed under the MIT License.

## Related Projects

1. related project [SYSTRAN/faster-whisper](https://github.com/SYSTRAN/faster-whisper)
2. related project [m-bain/whisperX](https://github.com/m-bain/whisperX)