# Garou: Mark of the Wolves — ROM Patches

This repository contains BPS patches for **Garou: Mark of the Wolves**, all targeting the **P4 ROM** (`253-ep4.p4`). They fix and improve the game's Training Mode, and restore SNK's unused "FATAL FURY" branding on USA machines.

| Patch | What it does | Source ROM it expects |
|---|---|---|
| `garou_all_patches.bps` | **All four fixes below, in one patch** | Clean, vanilla P4 ROM |
| `garou_training_music_patch.bps` | Restores background music in Training Mode, with an ON/OFF toggle | Clean, vanilla P4 ROM |
| `garou_no_pause_patch.bps` | Fixes leftover "PAUSE" text corrupting the Training Mode options menu | Clean, vanilla P4 ROM |
| `garou_stage_variations_patch.bps` | Lets you pick alternate stage variations (day / sunset / night, etc.) in STAGE CHANGE | Clean, vanilla P4 ROM |
| `garou_fatal_fury_title_patch.bps` | Restores the unused "FATAL FURY" title screen and logo — **USA region only** | Clean, vanilla P4 ROM |

Pick the one that fits what you want:

- **Everything** → apply `garou_all_patches.bps` to a clean ROM. This is the recommended option.
- **Just one fix** → apply that single patch to a clean ROM.

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

### Checksums

Source `253-ep4.p4` — CRC32 `DA92C08E`

| Patch | Patched P4 CRC32 |
|---|---|
| `garou_all_patches.bps` | `B1D1A6EE` |
| `garou_training_music_patch.bps` | `F9617DE9` |
| `garou_no_pause_patch.bps` | `1723FD9F` |
| `garou_stage_variations_patch.bps` | `2062C120` |
| `garou_fatal_fury_title_patch.bps` | `41E98056` |

Your emulator or ROM manager will warn that the P4 ROM's checksum no longer matches. That is expected for a patched ROM.

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

- **ON** — Training Mode music isº restored and follows the selected stage.
- **OFF** — Training Mode music is muted.

<img width="960" height="672" alt="garou_music_menu" src="https://github.com/user-attachments/assets/70b096b0-df51-48a1-9da8-bed3a3773647" /> 

### PAUSE fix patch

Fixes a bug in the original game where reopening the Training Mode options menu (pressing Select a second time, after closing and reopening it during a training session) shows corrupted, garbled text below "PRESS SELECT TO CONTINUE."

The corruption is literally leftover "PAUSE" text — the same text shown by the game's normal single-player pause screen — bleeding through into the Training Mode menu, which isn't supposed to show it at all. The patch adds a small check that runs once per frame: while in Training Mode, it detects and blanks out that specific leftover text before it reaches the screen.

This only affects Training Mode. The normal single-player PAUSE screen (Story/Survival/VS modes) is untouched and still displays exactly as before.

### Stage variations patch

Several Garou stages have more than one version. During a normal match the game swaps between them from round to round — Gato's stage for example goes from sunset to night, and a few stages change so much they read as an entirely different place. In Training Mode you only ever got the first version of each stage, because Training Mode has no rounds for the game to count.

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

<img width="960" height="672" alt="garou_stage_variations" src="https://github.com/user-attachments/assets/08bc7584-a36a-4677-95a3-49f68aca0b3b" />

### FATAL FURY title patch

Garou was going to be released in the US as **"Fatal Fury: Mark of the Wolves"**. SNK left the artwork in the ROM but never wired it up, so every region shows the Japanese 〔餓狼〕 branding.

This patch restores the English branding in two places:

- **The winged title screen** — the one shown at the end of the second attract-mode intro. It now reads **FATAL FURY / MARK OF THE WOLVES**, including the full logo fade-in animation, which SNK also left in the ROM complete and unused.
- **The small corner logo** — the winged mark shown in the corner during matches and DEMONSTRATION. SNK left a complete English version of this too, sitting right next to the Japanese one.

**The patch is region-gated.** It reads the BIOS country code and only takes effect on USA machines:

| Region | Winged title screen | In-game corner logo |
|---|---|---|
| **USA** | FATAL FURY / MARK OF THE WOLVES | FATAL FURY / MARK OF THE WOLVES |
| Japan | 〔餓狼〕GAROU — unchanged | 〔餓狼〕 — unchanged |
| Europe | 〔餓狼〕GAROU — unchanged | 〔餓狼〕 — unchanged |

On Japanese and European machines the game behaves exactly as the original. On a Universe BIOS, set the region to USA in the BIOS menu.

Nothing else is touched. The first intro's plain title screen and all other in-game graphics are left exactly as they were in every region.

## Credits

Thanks to **DaRKSLaiN** ([@sete_kitt](https://x.com/sete_kitt)) for the idea of making the alternative stage versions selectable in Training Mode.

## Notes

All patches only modify the **P4 ROM**. Keep the rest of the Garou ROM set unchanged.

Keep a backup of the original ROM and verify that each patched ROM has the expected size for your setup.
