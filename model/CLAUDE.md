# model/ (humming transcription model)

Owners: Shawnak (Modal and data), Bingchang (training), Alex (evaluation).

## Goal
A PyTorch model trained on humming that replaces Basic Pitch in spring. It must return the same note format: `[{"pitch": 60, "onset": 0.42, "offset": 0.79}]` (MIDI number, seconds). Trained on Modal GPUs.

## Paper and the no-copy rule
- We follow the Dynamic HumTrans paper (arXiv 2410.05455).
- Its code repo (github.com/shubham-gupta-30/humming_transcription) has **no license**. Read it, write our own version. Never copy code from it into this repo.

## Data: HumTrans
- huggingface.co/datasets/dadinghh2/HumTrans
- 14,614 hummed clips, about 56 hours, each with the MIDI of its melody.
- License CC BY-NC 4.0 (non-commercial).
- Lives on a Modal Volume named `humtrans`.
- Nobody downloads the 15 GB zip to a laptop. Pull single clips with the `remotezip` library.
- Our model uses 44,100 Hz audio.

## Corrected timings
- HumTrans's MIDI timings don't line up with the audio.
- Score against the corrected timings in the Dynamic HumTrans repo's `heurestic_filtered_onsets_offsets/` folder, never the raw MIDI timings.

## Evaluation (always pass these; mir_eval's defaults are different)
- Note score: `mir_eval.transcription.precision_recall_f1_overlap(..., onset_tolerance=0.05, pitch_tolerance=1.0, offset_ratio=None)`
- Pitch-only score: same call with `onset_tolerance=5.0`

## Checkpoints
- About 360 MB each. GitHub rejects files over 100 MB.
- Checkpoints never go in git (`*.pt`, `*.ckpt`, `checkpoints/` are gitignored). Keep them on Modal.
