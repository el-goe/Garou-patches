# Garou: Mark of the Wolves — Training Mode Patches

This repository contains BPS patches for **Garou: Mark of the Wolves**, all targeting the **P4 ROM**. Together they fix and improve the game's Training Mode.

| Patch | What it does | Source ROM it expects |
|---|---|---|
| `garou_training_music_patch.bps` | Restores background music in Training Mode, with an ON/OFF toggle | Clean, vanilla P4 ROM |
| `garou_no_pause_patch.bps` | Fixes leftover "PAUSE" text corrupting the Training Mode options menu | Clean, vanilla P4 ROM |
| `garou_no_pause_on_music_patch.bps` | Same PAUSE fix as above, for use **together with** the music patch | P4 ROM after the music patch has been applied |

Pick the combination that fits what you want:

- **Just the music patch** → apply `garou_training_music_patch.bps` to a clean ROM. Done.
- **Just the PAUSE fix** → apply `garou_no_pause_patch.bps` to a clean ROM. Done.
- **Both** → apply the music patch first, then apply `garou_no_pause_on_music_patch.bps` (not `garou_no_pause_patch.bps`) to that result. See [Applying both patches together](#applying-both-patches-together).

## Requirements

- A clean, matching **Garou: Mark of the Wolves P4 ROM**
- The patch file(s) you want from this repository
- A BPS patching tool, such as **Floating IPS (Flips)**

## Applying a single patch

1. Back up your original P4 ROM.
2. Open your BPS patching tool and choose **Apply Patch**.
3. Select either `garou_training_music_patch.bps` or `garou_no_pause_patch.bps`.
4. Select the clean Garou P4 ROM as the source.
5. Save the patched P4 ROM.

## Applying both patches together

The two fixes are separate patches with independent trampoline code, so combining them requires applying them in sequence rather than picking both files against a clean ROM:

1. Back up your original P4 ROM.
2. Apply `garou_training_music_patch.bps` to the clean P4 ROM. Save the result — this is your music-patched ROM.
3. Apply `garou_no_pause_on_music_patch.bps` **to the music-patched ROM** from step 2 (not the original clean ROM, and not using `garou_no_pause_patch.bps`).
4. Save the result. This final ROM has both fixes.

Each patch checks the source ROM's checksum and will refuse to apply (or warn) if it doesn't match what it expects — see the table above for which source each patch needs. In particular, `garou_no_pause_patch.bps` and `garou_no_pause_on_music_patch.bps` are two different builds of the same fix for two different starting points; don't mix them up.

## What each patch does

### Music patch

Restores Training Mode music when:

- Entering Training Mode.
- Changing the stage.
- Changing characters and returning to the training fight.
- Returning characters to the center of the stage.

The music selection follows the current stage.

It also adds a new entry in the Training Mode options menu that lets you enable or disable the music restoration.

Enter Training Mode and use the new **MUSIC** option in the Training Mode menu to toggle the feature.

- **ON** — Training Mode music is restored and follows the selected stage.
- **OFF** — Training Mode music is muted.

<img width="960" height="672" alt="garou_music_menu" src="https://github.com/user-attachments/assets/70b096b0-df51-48a1-9da8-bed3a3773647" /> 

### PAUSE fix patch

Fixes a bug in the original game where reopening the Training Mode options menu (pressing Select a second time, after closing and reopening it during a training session) shows corrupted, garbled text below "PRESS SELECT TO CONTINUE."

The corruption is literally leftover "PAUSE" text — the same text shown by the game's normal single-player pause screen — bleeding through into the Training Mode menu, which isn't supposed to show it at all. The patch adds a small check that runs once per frame: while in Training Mode, it detects and blanks out that specific leftover text before it reaches the screen.

This only affects Training Mode. The normal single-player PAUSE screen (Story/Survival/VS modes) is untouched and still displays exactly as before.

This fix is available as two separate patch files with identical behavior — `garou_no_pause_patch.bps` for a clean ROM, and `garou_no_pause_on_music_patch.bps` for a ROM that already has the music patch applied. Use whichever matches your starting ROM.

## Notes

All patches only modify the **P4 ROM**. Keep the rest of the Garou ROM set unchanged.

Keep a backup of the original ROM and verify that each patched ROM has the expected size for your setup.
