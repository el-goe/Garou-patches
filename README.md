# Garou: Mark of the Wolves — ROM Patches

This repository contains BPS patches for **Garou: Mark of the Wolves**, all targeting the **P4 ROM** (`253-ep4.p4`). They fix and improve the game's Training Mode, restore SNK's unused "FATAL FURY" branding on USA machines, and clean up the flickering floor shadows.

| Patch | What it does | Source ROM it expects |
|---|---|---|
| `garou_all_patches.bps` | **All five fixes below, in one patch** | Clean, vanilla P4 ROM |
| `garou_training_music_patch.bps` | Restores background music in Training Mode, with an ON/OFF toggle | Clean, vanilla P4 ROM |
| `garou_no_pause_patch.bps` | Fixes leftover "PAUSE" text corrupting the Training Mode options menu | Clean, vanilla P4 ROM |
| `garou_stage_variations_patch.bps` | Lets you pick alternate stage variations (day / sunset / night, etc.) in STAGE CHANGE | Clean, vanilla P4 ROM |
| `garou_fatal_fury_title_patch.bps` | Restores the unused "FATAL FURY" title screen and logo — **USA region only** | Clean, vanilla P4 ROM |
| `garou_portrait_patch.bps` | Shows the sparring character's portrait while you pick it in Training Mode | Clean, vanilla P4 ROM |
| `garou_shadow_colors.bps` | Removes the flicker from the floor shadows and gives each stage its own shadow colour | A ROM already patched with `garou_all_patches.bps` |

Pick the one that fits what you want:

- **Everything** → apply `garou_all_patches.bps` to a clean ROM, then `garou_shadow_colors.bps` on top. This is the recommended option.
- **Just one fix** → apply that single patch to a clean ROM.

Every patch here except `garou_shadow_colors.bps` is applied to a **clean, unmodified P4 ROM**, and you pick exactly one of them.

The individual patches each place their own code in the same spare area of the ROM, so they cannot be stacked on top of one another — applying a second one to an already-patched ROM will not work. If you want more than one fix, use `garou_all_patches.bps`, which is a single combined build with that spare space divided up properly.

`garou_shadow_colors.bps` is the one exception, and it works the other way round: it is **built to be applied on top of `garou_all_patches.bps`**, not to a clean ROM. It uses a separate spare area that the combined build leaves untouched, and it chains onto the combined build's own frame hook instead of replacing it. Apply `garou_all_patches.bps` first, then apply `garou_shadow_colors.bps` to the result.

## Requirements

- A clean, matching **Garou: Mark of the Wolves P4 ROM**
- One patch file from this repository, plus `garou_shadow_colors.bps` if you want the shadow fixes
- A BPS patching tool, such as **Floating IPS (Flips)**

## Applying a patch

1. Back up your original P4 ROM.
2. Open your BPS patching tool and choose **Apply Patch**.
3. Select the patch file you want.
4. Select the clean Garou P4 ROM as the source.
5. Save the patched P4 ROM.

To add the shadow patch, repeat those steps with `garou_shadow_colors.bps`, using
the ROM you just saved as the source rather than a clean one.

Each patch checks the source ROM's checksum and will refuse to apply (or warn) if it doesn't match. If you get a checksum mismatch, you're almost certainly feeding it the wrong source — every patch except `garou_shadow_colors.bps` wants a clean ROM, and that one wants the output of `garou_all_patches.bps`. Start again from a clean backup.

### Checksums

Source `253-ep4.p4` — CRC32 `DA92C08E`

| Patch | Patched P4 CRC32 |
|---|---|
| `garou_all_patches.bps` | `B048742A` |
| `garou_training_music_patch.bps` | `F9617DE9` |
| `garou_no_pause_patch.bps` | `1723FD9F` |
| `garou_stage_variations_patch.bps` | `2062C120` |
| `garou_fatal_fury_title_patch.bps` | `41E98056` |
| `garou_portrait_patch.bps` | `DB0B124A` |

The shadow patch is applied to an already-patched ROM, so it has a different source:

| Patch | Source P4 CRC32 | Patched P4 CRC32 |
|---|---|---|
| `garou_shadow_colors.bps` | `B048742A` (output of `garou_all_patches.bps`) | `EAF6D7C3` |

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

- **ON** — Training Mode music is restored and follows the selected stage.
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

<img width="1280" height="896" alt="garou_alt_title" src="https://github.com/user-attachments/assets/eb57b70a-6bce-4c39-b6d7-5d3663697390" />

### Portrait patch

In Training Mode you pick your own character first, and then a second one to
spar against. The game shows your character's portrait on the left, but the
right-hand panel stays empty for the whole of the second choice — the only
feedback you get is the small cursor moving along the row of face icons at the
bottom of the screen.

This patch fills that panel in. While you are choosing the sparring character,
its portrait appears in the P2 slot and follows the cursor, exactly the way P2's
portrait behaves in VS Mode.

It is most useful for the hidden characters. Grant and Kain are not part of the
normal roster, and their icons in the selection row give you very little to go
on, so picking one as your sparring partner was largely guesswork — you found
out which one you had chosen after the fight had already loaded. With the
portrait showing, you can see exactly who you are about to select.

The patch only changes the second choice in Training Mode's character select.
Your own character's portrait, the VS Mode selection screen and every other mode
are untouched.

### Shadow de-flicker and colours patch

On most stages the characters cast a shadow on the floor: a copy of the character
sprite, flipped vertically and drawn in solid black. The Neo Geo has no
transparency, so the game draws each shadow only on every other frame and lets a
CRT average it out to roughly half strength. The two shadows run in opposite
phase, so only one of them is ever on screen at any moment. On a CRT this reads
as a soft, semi-transparent shadow. On anything else it reads as flicker.

This patch does two things:

**Removes the flicker.** Both shadows are now drawn every frame. The change is a
single branch in the shadow routine, which the game used to take on alternate
frames to skip the draw.

**Gives each stage its own shadow colour.** Drawn every frame, a shadow that was
designed to be seen half the time comes out far too dark, so a flat black no
longer looks right. The patch assigns a colour per stage, and per stage version
where the versions differ enough to need it, chosen to sit against that stage's
floor the way the half-strength original did.

| Stage | Shadow colour | | Stage | Shadow colour |
|---|---|---|---|---|
| TERRY | none — no shadows | | MARCO | olive |
| ROCK | cool grey-teal | | HOKUTOMARU | warm red-brown |
| DONGHWAN | dark red | | FREEMAN | none — no shadows |
| JAEHOON | blue-violet | | GRIFFON | neutral grey |
| HOTARU 1 | near-black blue | | KEVIN 1 / 2 | deep red |
| HOTARU 2 | lighter blue | | KEVIN 3 | cold blue |
| GATO | none — no shadows | | GRANT | warm brown |
| B.JENET | slate | | KAIN | cold blue |

Terry and Freeman stages have no floor shadows at all. Gato's stage uses a rippling 
water effect rather than a shadow; both keep as the original game.

The colours live in a table inside the patch, one entry per stage and version,
so they can be retuned later without touching any of the surrounding code.

Nothing else changes. The shadows behave identically in every mode — Story,
Survival, VS and Training all get the de-flickered, coloured shadows.

<img width="640" height="1568" alt="garou_colors" src="https://github.com/user-attachments/assets/54275fff-11ef-4d36-930c-365dfc6ce15b" />


## Credits

Thanks to **DaRKSLaiN** ([@sete_kitt](https://x.com/sete_kitt)) for the idea of making the alternative stage versions selectable in Training Mode and **Víctor** ([@vcp84](https://x.com/vcp84)) for reminding me the existence of the unused Fatal Fury title screen and attract mode logo.

## Notes

All patches only modify the **P4 ROM**. Keep the rest of the Garou ROM set unchanged.

Because the shadows were originally meant to be seen half the time, drawing both
of them every frame puts more sprites on the same scanlines than the original
game ever did. Nothing was observed to drop out in testing, but the Neo Geo has a
hard per-scanline sprite limit and real hardware may behave differently from an
emulator here.

Keep a backup of the original ROM and verify that each patched ROM has the expected size for your setup.
