# Ethan Sprite Fix — PIL Task

## Goal
Repaint Ethan's hair, hat, and glasses directly onto his sprite PNG using Python + Pillow.
The result should be saved back to `assets/ethan_sprite.png`.

## Source file
Work from: `assets/ethan_sprite_original.png`
Size: 354×1197 px, RGBA
Do NOT modify the original — write output to `assets/ethan_sprite.png`

## Reference files
- `assets/ethan_reference.png` — **PHOTO OF REAL ETHAN** — use this as the visual ground truth
- `assets/ethan_sprite_original.png` — clean base (no hat/hair mods)
- `assets/ethan_sprite_original2.png` — intermediate version (has cap + glasses already baked, hair still partially curly)
- `assets/ethan_sprite.png` — current output (latest attempt, replace this)
- `ethan_original_view.html` — open in browser to compare all three side by side

## Coordinate system
The sprite represents a full-body cartoon character.
- Feet are at the **bottom** of the image (y = 1197)
- Head top is near y ≈ 60–80
- Face/hair region: roughly y = 80–350
- Character is centered horizontally around x = 177

Sprite pixel ↔ game unit conversion (for reference only, not needed for PIL work):
```
scale = sprite_height / 175   # 1197 / 175 ≈ 6.84
sprite_x = 177 + game_x * scale
sprite_y = 1197 + game_y * scale   # game_y is negative upward
```

## What Ethan should look like
See `assets/ethan_reference.png` for the real person.

### Hair
- **Style**: Very long, straight, dark brown — hangs well past the shoulders, loose
- **Color**: Dark brown `#3a2010` with midtone highlights `#5a3520`
- **Coverage**: Falls from under the cap on both sides of the face and continues down the back/sides well below shoulder level
- Hair panels must be wide enough to fully cover any original curly/wavy hair showing through — blank those areas with skin tone first

### Hat
- **Style**: Backward black baseball cap — brim points BEHIND the head, NOT over the face
- **Cap body**: Sits on top of the head, fairly low profile, slightly tilted back
- **Colors**: Cap body `#111111`, brim `#1a1a1a`, band `#0a0a0a`
- **Snap tab**: Small rectangle visible at the front center (which faces forward since brim is backward)

### Glasses
- **Style**: Round/oval dark sunglasses — amber or very dark tint
- **Lens color fill**: `(30, 15, 5, 200)` — dark amber-brown, slight transparency
- **Frame color**: `#0d0d0d`
- **Position**: Sit on the nose, centered on the face
- **Size**: Round, radius ≈ 18–22 px at sprite scale
- **Bridge**: Short horizontal line connecting lenses
- **Temples**: Lines extending from outer lens edges toward ears

### Beard
- **Style**: Short trimmed beard — fuller than stubble, covers jaw and upper lip
- **Color**: Dark brown, slightly lighter than hair `#5a3520` base with `#3a2010` shadow
- **Coverage**: Jawline, chin, mustache area

### Earring
- Small stud in the left ear — tiny white/silver dot

### Build
- Ethan is the **tallest** character — his `_h` value in the game should be the largest (suggest 185 vs default 175)

## What to avoid
- Do NOT leave any of the original sprite's curly/wavy hair visible — blank it with skin tone first
- Do NOT place the cap brim over the face (brim goes backward)
- Do NOT use a beanie — it must be a baseball cap with a brim
- Do NOT use the old overlay system — paint directly into the PNG pixels

## Skin tone reference
Sample the face area of `assets/ethan_sprite_original.png` at approximately (177, 200) to get the exact skin color, then use that to blank any hair regions before repainting.

## Output
Save final result to: `assets/ethan_sprite.png`
Print confirmation: filename + dimensions when done.
