# NARA'S ROOM — HALF COURT HERO

## Overview
A 2v2 half-court basketball game featuring characters from the "Nara's Room" universe, styled with a nostalgic "Lizzy McGuire" cartoon aesthetic.

## Authenticity & Source Material
The game is a direct tie-in to the "Nara's Room - Lizzie McGuire" official music video. The character designs and roles are meticulously mapped from the band members' appearances in the video:

- **Nara (Vocals/Guitar):** Matches her cartoon avatar in the video with the signature black bob, black t-shirt, and blue shorts.
- **Ethan (Bass):** Features his iconic lime green, black, and white patterned jersey and sunglasses.
- **Brendan (Drums):** Wears a white "WMPG 90.9" graphic tee over orange long sleeves, complete with a grey beanie.
- **Will (Keys):** Identified by his striped brown/grey shirt over grey sleeves, beanie, and glasses.

## Findings
- **Technology Stack:** Vanilla HTML5 Canvas API, JavaScript (ES6+), and CSS. Zero external dependencies.
- **Game Mechanics:** 
  - **Shooting:** Uses a timing-based "shot ring" mechanic for accuracy.
  - **Defense:** Includes both blocking and stealing mechanics.
  - **NPC Behavior:** Team B (Brendan & Will) is controlled by a state-based system that drives to the hoop and shoots.
  - **Controls:** Multi-modal support (Touch, Keyboard `Arrow/WASD + Z/X`, and Mouse).
- **Architecture:** 
  - Centralized state management via a global object `S`.
  - Frame-based animation loop using `requestAnimationFrame`.
  - Procedural background rendering combined with sprite-based characters.

## Critical Analysis

### Strengths
1. **Art Direction:** Excellent execution of the Y2K-era cartoon style, including the use of "quips" and character-specific color palettes.
2. **Input Responsiveness:** The dual joystick/button layout for mobile is well-implemented and feels responsive.
3. **Skill Ceiling:** The shot ring adds a layer of depth beyond simple button-mashing, rewarding precise timing.
4. **Efficiency:** Highly optimized single-file codebase that runs smoothly on both mobile and desktop browsers.

### Opportunities for Improvement
1. **Audio Integration:** The game lacks sound. Adding bubble-pop sounds for quips, a squeaky court sound, and a Y2K pop-rock soundtrack would complete the experience.
2. **Physics Fidelity:** The ball-to-rim collision logic is somewhat rigid. Implementing more varied bounce vectors could make rebounds feel more natural.
3. **Bot Coordination:** Currently, NPC teammates move independently. Adding basic "plays" (e.g., pick and roll or cutting to the basket) would increase difficulty.
4. **Maintainability:** At 1,400+ lines, the single-file structure is reaching its limit. Refactoring into a modular structure (e.g., `physics.js`, `render.js`, `cpu.js`) would benefit future expansion.

## Repository Structure
- `index.html`: The core game logic and UI.
- `assets/`: Contains character sprites (`nara`, `ethan`, `brendan`, `will`) and the court environment.
- `screenshots/`: Promotional and gameplay captures of various game states.
- `.wrangler/`: Configuration for Cloudflare Pages deployment.
