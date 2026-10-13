# HumScore

A Georgia Tech WebDev club project (Fall 2026 – Spring 2027). You hum a melody into a laptop or phone mic and the app turns it into sheet music you can see, play back, and download as MusicXML, MIDI or PDF. Editing notes on the score comes in spring.

## Pipeline
1. **Record** (browser, `web/`): record the mic as WAV at the browser's own sample rate, POST it to the server's `/transcribe`. A click track can set the tempo.
2. **Detect** (server, `server/`): find each note's pitch, start and end. Fall: Spotify's Basic Pitch (`basic-pitch`, Apache 2.0). Spring: our own PyTorch model trained on humming (`model/`). Both return the same note format, so nothing downstream changes on the swap.
3. **Notate** (server): snap notes to a beat grid using the tempo, guess the key, add barlines, write MusicXML with music21.
4. **Use it** (browser): render MusicXML with OpenSheetMusicDisplay, play with Tone.js, export.

## The note format (team contract)
`/transcribe` returns a JSON list:
```json
[{"pitch": 60, "onset": 0.42, "offset": 0.79}]
```
- `pitch`: MIDI note number (60 = middle C)
- `onset`, `offset`: seconds from the start of the recording
- TypeScript type: `web/src/types/note.ts`

## Stack
- `web/`: React, TypeScript, Vite, Tailwind CSS v4, OpenSheetMusicDisplay, Tone.js
- `server/`: Python 3.11, FastAPI, deployed on Modal. Supabase (login, Postgres, file storage) comes later.
- `model/`: PyTorch, trained on Modal GPUs with the HumTrans dataset

Each folder has its own CLAUDE.md with details.

## Who owns what
- `web/`: Aadi (app structure, recording, integration), Ben (score view)
- `server/`: Anna (API on Modal), Valerie (Notate step: rhythm, key, MusicXML)
- `model/`: Shawnak (Modal and data), Bingchang (training), Alex (evaluation)
- PM: Kushagra

## How we work
- Tickets are GitHub issues on the org's project board.
- Every change goes through a pull request whose description says `Closes #<issue number>`. `main` is protected.
- The repo is public.
- Keep code simple and readable; most of the team is new to this stack.

## Rules
- Never commit audio, datasets, checkpoints or `.env` files.
- Never copy code from the Dynamic HumTrans repo (github.com/shubham-gupta-30/humming_transcription). It has no license. Read it, write our own.
- Don't change the note format without the whole team agreeing.
- Keep changes small and inside the folder you own.
- Explain any new dependency in the PR.
