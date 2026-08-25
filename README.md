# Garou: Mark of the Wolves — Training Mode Patches

This repository contains BPS patches for **Garou: Mark of the Wolves**, all targeting the **P4 ROM**. Together they fix and improve the game's Training Mode.

| Patch | What it does | Source ROM it expects |
|---|---|---|
| `garou_all_patches.bps` | **All three fixes below, in one patch** | Clean, vanilla P4 ROM |
| `garou_training_music_patch.bps` | Restores background music in Training Mode, with an ON/OFF toggle | Clean, vanilla P4 ROM |
| `garou_no_pause_patch.bps` | Fixes leftover "PAUSE" text corrupting the Training Mode options menu | Clean, vanilla P4 ROM |
| `garou_stage_variations_patch.bps` | Lets you pick alternate stage variations (day / sunset / night, etc.) in STAGE CHANGE | Clean, vanilla P4 ROM |

Pick the one that fits what you want:

- **Everything** → apply `garou_all_patches.bps` to a clean ROM. This is the recommended option.
- **Just the music patch** → apply `garou_training_music_patch.bps` to a clean ROM.
- **Just the PAUSE fix** → apply `garou_no_pause_patch.bps` to a clean ROM.
- **Just the stage variations** → apply `garou_stage_variations_patch.bps` to a clean ROM.

Every patch here is applied to a **clean, unmodified P4 ROM**. Pick exactly one and you're done.

The individual patches each place their own code in the same spare area of the ROM, so they cannot be stacked on top of one another — applying a second one to an already-patched ROM will not work. If you want more than one fix, use `garou_all_patches.bps`, which is a single combined build with that spare space divided up properly.

## Requirements

- A clean, matching **Garou: Mark of the Wolves P4 ROM**
- One patch file from this repository
- A BPS patching tool, such as **Floating IPS (Flips)**

## Applying a patch

1. Back up your original P4 ROM.
2. Open your BPS patching tool and choose **Apply Patch**.
3. Select the patch file you want.
4. Select the clean Garou P4 ROM as the source.
5. Save the patched P4 ROM.

Each patch checks the source ROM's checksum and will refuse to apply (or warn) if it doesn't match. If you get a checksum mismatch, you're almost certainly feeding it a ROM that has already been patched — start again from a clean backup.

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

### Stage variations patch

Several Garou stages have more than one version. During a normal match the game swaps between them from round to round — Terry's harbour goes from sunset to night, and a few stages change so much they read as an entirely different place. In Training Mode you only ever got the first version of each stage, because Training Mode has no rounds for the game to count.

This patch extends the **STAGE CHANGE** option so every version is selectable directly. The option now lists 30 entries instead of 14, with a number after the stage name where more than one version exists:

| Stage | Versions | | Stage | Versions |
|---|---|---|---|---|
| TERRY | 3 | | MARCO | 2 |
| ROCK | 1 | | HOKUTOMARU | 3 |
| DONGHWAN | 1 | | FREEMAN | 1 |
| JAEHOON | 3 | | GRIFFON | 3 |
| HOTARU | 2 | | KEVIN | 3 |
| GATO | 3 | | GRANT | 1 |
| B.JENET | 3 | | KAIN | 1 |

Stages with only one version are listed by name alone, exactly as before. Left and right on the STAGE CHANGE row cycle through the full list, and the selected version loads immediately when you close the menu.

This only affects Training Mode. Story, Survival and VS still rotate stage versions between rounds exactly as the original game does.

## Credits

Thanks to **DaRKSLaiN** ([@sete_kitt](https://x.com/sete_kitt)) for the idea of making the alternative stage versions selectable in Training Mode.

## Notes

All patches only modify the **P4 ROM**. Keep the rest of the Garou ROM set unchanged.

Keep a backup of the original ROM and verify that each patched ROM has the expected size for your setup.