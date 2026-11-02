# AI Audio Transcriber

An audio-to-document pipeline with local speech recognition, agent-based transcript
cleanup, summaries, multilingual translation, and a Gradio interface.

This is my portfolio fork of the [team project](https://github.com/kennionblack/ai-audio-transcriber).
The original Git history and MIT license are preserved. My contributions include
the cleaner agent, frontend integration, event-driven UI updates, transcript lookup,
PDF downloads, and the multilingual interface.

## What it does

- Transcribes audio locally using faster-whisper on the CPU.
- Coordinates transcript cleanup and bullet-point summaries through agent tools.
- Streams structured progress and results into a Gradio web interface.
- Translates transcripts and summaries into supported target languages.
- Exports results to JSON, DOCX, and PDF.
- Provides both a browser interface and command-line workflow.

## Architecture

| Layer | Implementation |
| --- | --- |
| Speech recognition | faster-whisper, multilingual base model, CPU/int8 |
| Agent orchestration | Python, YAML configuration, registered tool functions |
| Cleanup and translation | OpenAI API |
| Interface | Gradio with subprocess event streaming |
| Shared state | Validated transcript, summary, and metadata context |
| Documents | JSON, python-docx, fpdf2 |

## Run locally

Requires Python 3.12+, ffmpeg, and your own OpenAI API key.

```sh
python -m venv .venv
# Windows PowerShell:
.\.venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Set your own key in the ignored `.env` file. On Windows, `Copy-Item .env.example .env`
can replace `cp`. Then launch the interface:

```sh
python gradio_app.py
```

Open http://localhost:7860. For the command-line workflow:

```sh
python agent.py path/to/audio.mp3
python agent.py -t es path/to/audio.mp3
```

Supported translation languages: English, Chinese, French, Spanish, German,
Japanese, Korean, Portuguese, Arabic, and Russian. The first transcription run
may download a Whisper model. Results are written under `output/`.

## Data handling

Speech recognition runs locally. Transcript text is sent to the configured
OpenAI service for cleanup, summaries, and translation. API usage may incur charges.
Upload only audio you have permission to process. Local secrets, uploaded audio,
generated output, and runtime logs are excluded from this fork's new commits.

## Development

```sh
pip install -r requirements-dev.txt
pytest
```

The full test suite includes a real speech-recognition test that may download a
model. To run only the mocked and formatting tests without live API calls:

```sh
pytest tests/test_agents.py -k "not deterministic_output_for_audio_file"
```

See [the original setup guide](docs/original-setup.md) for container development,
CLI options, and the team's original pipeline notes.

## Project status

This is a working team-project snapshot with ongoing development. This fork
highlights my multilingual UI branch; the upstream project may have newer changes.
No hosted demo or deployment is configured for this portfolio fork.
