# AggroBark-Dataset-Generator

Generate a 1,000-clip synthetic dataset of aggressive dog barking using the ElevenLabs Sound Effects API.

## Description

AggroBark-Dataset-Generator is a Python script that builds a labeled audio dataset of aggressive dog barks. It creates 1,000 unique text prompts from a grid of breed, intensity, distance, environment, and dog count, sends each one to the ElevenLabs Sound Effects API, and saves the audio along with a metadata file recording the prompt used for every clip.

Useful for:
- Training or testing dog-bark / aggressive-bark classifiers
- Audio event detection research
- Prototyping pet-monitoring or security-alert systems

## Features

- 1,000 unique prompts built from a configurable grid
- Barking-only prompts (no growls, whines, or howls)
- `metadata.csv` mapping each audio file to its prompt
- Resumable: skips clips that already exist
- Automatic retries on API errors and rate limits
- Optional ffmpeg conversion to 16 kHz mono WAV

## Requirements

- Python 3.8+
- An [ElevenLabs](https://elevenlabs.io) API key (a paid plan is likely needed for 1,000 clips)
- `requests` library
- (Optional) `ffmpeg` for WAV conversion

## Installation

```bash
git clone https://github.com/<your-username>/AggroBark-Dataset-Generator.git
cd AggroBark-Dataset-Generator
pip install requests
```

## Usage

1. Set your API key:

   ```bash
   # macOS / Linux
   export ELEVENLABS_API_KEY="your_key_here"

   # Windows (cmd)
   set ELEVENLABS_API_KEY=your_key_here
   ```

2. Run the generator:

   ```bash
   python generate_dog_bark_dataset.py
   ```

3. Find the output in the `dataset/` folder:

   ```
   dataset/
   ├── aggressive_bark_0001.mp3
   ├── aggressive_bark_0002.mp3
   ├── ...
   └── metadata.csv
   ```

> Tip: set `TOTAL_CLIPS = 10` at the top of the script and test first, since a full run uses a lot of credits.

## Configuration

Edit these values at the top of `generate_dog_bark_dataset.py`:

| Setting | Default | Meaning |
|---|---|---|
| `TOTAL_CLIPS` | 1000 | Number of clips to generate |
| `DURATION_SECONDS` | 4 | Length of each clip (0.5 to 22) |
| `PROMPT_INFLUENCE` | 0.5 | How strictly the model follows the prompt |
| `SEED` | 42 | Seed for reproducible prompt order |
| `BREEDS`, `INTENSITY`, `DISTANCE`, `ENVIRONMENT`, `COUNT` | see file | Prompt grid values |

## Convert to WAV (optional)

```bash
cd dataset
for f in *.mp3; do ffmpeg -i "$f" -ar 16000 -ac 1 "${f%.mp3}.wav"; done
```

## Tips for better datasets

- Listen to samples from each prompt type, since AI generators sometimes add growls or background noise.
- Mix in real recordings (e.g., Freesound, AudioSet) to improve real-world accuracy.
- Add augmentation (noise, pitch shift, reverb) before training.
- If training a classifier, generate a second class of non-aggressive barks with the same prompt grid.

## Licensing and terms

Audio is generated with ElevenLabs. Review their [terms of service](https://elevenlabs.io/terms), including the restriction on using output to develop competitive products or services, before using the dataset commercially or for model training.

## License

MIT License. See `LICENSE` for details. (The license covers the code only, not generated audio.)
