markdown
==============================================
X-CHANGE LIFE (XCL) COMPLETE MODDING GUIDE
==============================================

Last Updated: February 2025
Game Version: X-Lowe
Repository: https://gitgud.io/xchange-life/xcl-twee-xlowe
Official Wiki: https://x-change.life/wiki/
Official DISCORD: https://discord.gg/B2g2YYFn2N

==============================================
TABLE OF CONTENTS
==============================================

1. Navigation Macros
2. Choice Systems
3. Character & NPC Systems
4. Memory System
5. Activities & Gameplay
6. Media & UI
7. Data & Utilities
8. Modding Tools
9. Stats & Checks
10. Economy
11. Mood & Status
12. Wardrobe
13. Audio
14. Logic & Minigames
15. References & Links
16. Game Architecture & Structure
17. Advanced Modding Systems
18. Scripting & JavaScript Integration
19. Testing & Debugging
20. Community & Resources
21. Complete Workflow Example
22. Future-Proofing Your Mod
23. Appendix: Quick Reference
24. Final Notes

==============================================
1. NAVIGATION MACROS
==============================================

**(cs:)** - Change-screen navigation. Sets $next and replaces the center screen.
**Signatures:** `(cs: target, modeOrStage?)`
**Examples:** `(cs:"scene 04 page 2")`, `(cs:"Go to the bar","next")`

**(gt:)** - XCL wrapper for (goto:). Performs transition cleanup.
**Signatures:** `(gt: passage)`
**Examples:** `(gt:"day")`

**(screen:)** - Builds XCL three-panel layout.
**Signatures:** `(screen: right, center)`
**Examples:** `(screen:"character status","day")`

**($cs:)** - Convenience wrapper for (cs:).
**($nx:)** - Fullscreen navigation wrapper.
**($screen:)** - Deprecated, use (screen:) directly.
**($smart_screen:)** - Smart layout builder.

==============================================
2. CHOICE SYSTEMS
==============================================

**(simple_option:)** - Creates choice links with flags.
**Signatures:** `(simple_option: target, ...args)`
**Examples:** `(simple_option:"buy flowers","display","Daisies","Roses","Lilies")`

**(multi_option:)** - Renders multiple option groups.
**Signatures:** `(multi_option: ...options)`
**Examples:** `(multi_option:(a:"buy flowers","Daisies","Roses"), (a:"day","goto","Leave"))`

**($simple_option:)** - Alias of simple_option.
**($multi_option:)** - Wrapper for multi_option.
**($parse_option:)** - Low-level option builder.

**Flags:** "display", "goto", "next", "nostop", false

==============================================
3. CHARACTER & NPC SYSTEMS
==============================================

**Character Status Macros:**
- `(is_fem:)` - Returns true if female
- `(is_male:)` - Returns true if male
- `(is_preg:)` - Returns true if pregnant
- `(knows_preg:)` - Returns true if pregnancy known
- `(is_bim:)` - Returns true if "bimbo" effect
- `(is_pp:)` - Returns true if "people pleaser" effect
- `(pill: "name")` - Checks specific pill

**NPC Handling:**
- `($show_base_npc: "npc_id")` - Load NPC into UI
- `($char_passage:)` - Character-specific content
- `($bargirl_passage:)` - NPC-specific content
- `($passage_tags: "tag")` - Mod injection points

**Wardrobe:**
- `(outfitdb: "character", outfit_ids)` - Get outfits
- `(getoutfit: "outfit_key")` - Get outfit details

==============================================
4. MEMORY SYSTEM
==============================================

**Memory Structure:**
```javascript
{
  id: "unique_id",
  day: current_day,
  time: current_time,
  strength: days_until_expire,
  npc: "npc_id",
  media: "path/to/media",
  tags: ["tag1", "tag2"],
  location: "activity_location"
}
Core Memory Macros:

(remember: id, strength, npc, media, tags...) - Create memory

(remember_update: operation, id, args...) - Update memory

(recall: id, tag_checks...) - Recall memory

(forget: id) or (forget:) - Delete/prune memories

(memory_time: memory) - Natural language time

(days_since: "id") - Days since creation

(show_memory: memory, options) - Render memory

Example:

text
(remember: "stepsis shoplifting", 14, "stepsis", "none", "caught", "shoplifting")
(recall: "stepsis shoplifting", "not contains", "forgotten")
==============================================
5. ACTIVITIES & GAMEPLAY
==============================================

Gameplay Macros:

($set_mood: "mood", "reason") - Set character mood

($set_status: "status", "reason") - Add status effect

($register_orgasm:) - Log orgasm event

($gain_arousal: amount) - Gain arousal with styling

($core_female_status:) - Returns "bimbo" or "female"

Activities System:

Time-based progression

Location-specific (Mall, Gym, Bar, Home)

Stat modifications

Memory creation

Compulsions System:

Automatic behavior triggers

Tied to side effects (Bimbo, People Pleaser)

Can override player choices

Dreams System:

Add [dream] tag to passages

Add to $overnight_messages array

Use memory system for persistence

==============================================
6. MEDIA & UI
==============================================

Media Display:

(pic: image, format, class, options) - Display image

(vid: video, format, class, options) - Display video

(media: path_or_html, options) - Smart media (auto-detect)

($pic:), ($vid:) - Legacy wrappers

UI Components:

(svg-button: icon, size, state, colors, class) - Stylized SVG buttons

(floatnote: emoji, title, message, audio) - Floating notifications

(updateprogress: threshold, points, danger) - Progress bar

(updatemiddlebar: threshold, points, options) - Middle bar UI

(icon: "name", class, repeat) - Stat icons

(picker: "$var", options...) - Image picker

Tooltips:

(show_tooltip: content, tooltip) - Basic tooltip

(show_tooltip_wide: content, tooltip) - Wide layout

(show_tooltip_text: text, tip) - Text-only

(cleartooltips:), (tooltipgc:), (refreshtooltips:)

Text Styling:

($centered: [content]) - Centered block

($highlight: [content]) - Highlight span

($shadow: [content]) - Shadow text

($bimbo: [content]) - Bimbo-styled narration

($bimbo_dialogue: [content]) - Bimbo dialogue

($heading: "text") - Large heading

($caps: "text") - Capitalize words

==============================================
7. DATA & UTILITIES
==============================================

Randomness:

(newseed:) - Generate random seed

(twist: min, max) - Random integer

(twirl: values...) - Random value

(twerk: hooks...) - Random code execution

(twisted: values...) - Shuffle array

(either: values...) - Harlowe random choice

(shuffled: values...) - Harlowe shuffle

Math:

(average: values...) - Arithmetic mean

(rnd: number, precision) - Rounding

(clamp: value, min, max) - Clamp value

(sum: values...) - Sum numbers

(convert: value, from, to) - Unit conversion

Data Manipulation:

(dm: key1, value1, key2, value2...) - Create datamap

(check_dm: dm, key, operation, value) - Check datamap

(dmround: datamap, precision) - Round numeric values

(hash: value) - Generate hash

(indexof: array, item) - Find index

(intersection: a, b) - Array intersection

(remove: array, item, count) - Remove from array

Storage:

($get_session_storage: key, default) - Read sessionStorage

($get_local_storage: key, default) - Read localStorage

($set_session_storage: key, value) - Write sessionStorage

($set_local_storage: key, value) - Write localStorage

(savegameto: "slot") - Save game

(get_storage: "type", key, default) - Generic read

(set_storage: "type", key, value) - Generic write

Audio:

($play: type, track, delay) - Play audio tracks

Types: "sound", "ambience", "sex loop", "song", "scene", "story"

Debug:

(clearstandardvars:) - Reset engine variables

(del: "$var1", "$var2") - Delete variables

($delete_global_variable: "$var") - Reset global

($get_global: "$var", "js_expr") - Copy JS to Twine

($use_global: "$var", "js_expr", hook) - Temporary JS

($record_timing: "point") - Performance profiling

(win:) - Check if $result is "pass"

==============================================
8. MODDING TOOLS
==============================================

Essential Modding Macros:

($char_passage: base, core, fallback) - Character content

($bargirl_passage: base, core, fallback) - NPC content

($passage_tags: "tag") - Mod injection points

($show_base_npc: "npc_id") - Load NPC for mods/debug

Best Practices:

Use official macros for compatibility

Prefix custom variables: $_modname_variable

Test with base game updates

Use (display:) for UI refreshes

Clean up unused variables

Document mod requirements

==============================================
9. STATS & CHECKS
==============================================

Stat Checks:

($willpower_check: "passage", difficulty) - Fixed chance (101-difficulty)

($charm_check: "passage", difficulty) - Charm-based chance

($intellect_check: "passage", difficulty) - Intellect-based chance

($fitness_check: "passage", difficulty) - Fitness-based chance

Usage:

text
($charm_check: "flirt_success", 60)  # 60% success chance
($willpower_check: "resist", 40)     # 61% chance (101-40)
==============================================
10. ECONOMY
==============================================

Money Macros:

($gain_money: amount) - Add money with UI updates

($pay_money: amount) - Subtract money

(currency: amount) - Format as currency (USD)

Examples:

text
($gain_money: 50)
($pay_money: 25)
(print: (currency: 65))  # "$65"
==============================================
11. MOOD & STATUS
==============================================

Mood & Status Macros:

($set_mood: "mood", "reason") - Set mood (icon + buffs)

($set_status: "status", "reason") - Add status effect

Requirements:

Mood/status must exist in game tables

Automatically updates UI

Examples:

text
($set_mood: "humiliated", "because she rejected you")
($set_status: "cum breath", "Alexia came in your mouth")
==============================================
12. WARDROBE
==============================================

Wardrobe System:

(outfitdb: "character", outfit_ids) - Get outfits

(getoutfit: "outfit_key") - Get outfit details

Usage:

text
(set: $outfits to (outfitdb: "alexia"))
(set: $outfit to (getoutfit: "alexia casual sundress"))
==============================================
13. AUDIO
==============================================

Audio Playback:

($play: type, track, delay) - Play audio tracks

Audio Types:

"sound" - UI sound effects

"ambience" - Background sounds

"sex loop" - Loop music for scenes

"song" - Music tracks

"scene" - Scene-specific (folder shortcuts)

"story" - Storytelling audio

Examples:

text
($play: "sound", "ui good")
($play: "ambience", "indoors afternoon")
($play: "scene", "thwack", "2s")
==============================================
14. LOGIC & MINIGAMES
==============================================

Logic Macros:

(win:) - Returns true if $result is "pass"

($word_game_setup: dirty_talk_pairs) - Setup word minigame

Usage:

text
(set: $result to "pass")
(if: (win:))[You win!]

($word_game_setup: (a: "Line 1", "Sentence 1", "Line 2", "Sentence 2"))
==============================================
15. REFERENCES & LINKS
==============================================

Official Documentation:

Bar Girl System: https://x-change.life/wiki/docs/bar-girl/

Playable Character: https://x-change.life/wiki/docs/playable-character/

Activities System: https://x-change.life/wiki/docs/quick-guide-to-the-activities-system/

Custom Macros: https://x-change.life/wiki/docs/custom-x-change-life-macros-you-can-use-in-mods/

Creating Mods: https://x-change.life/wiki/docs/creating-mods/

Compulsions: https://x-change.life/wiki/docs/compulsions/

Memory System: https://x-change.life/wiki/docs/xcl-memory-system/

Adding Dreams: https://x-change.life/wiki/docs/adding-a-dream/

Code Repository:

https://gitgud.io/xchange-life/xcl-twee-xlowe

Harlowe Macros (Standard):

Variables: (set:), (put:), (move:)

Control Flow: (if:), (unless:), (else:), (for:), (loop:)

Data Structures: (a:), (dm:), (ds:), (range:), (find:)

TGP Macros (Also Available):

(indexofpage:), (indexofstatchange:), (and:), (not:)

(choice:), (resettopage:), (change_stat:), (read_stat:)

(notification:), (audiotoggle:), (playwhenread:), (map:)

==============================================
16. GAME ARCHITECTURE & STRUCTURE
==============================================

Game Folder Structure:

text
xcl-twee-xlowe/
├── img/                    # All media assets
│   ├── ui/                # Interface elements
│   │   ├── stats/         # Stat icons
│   │   └── icons/         # UI icons
│   ├── npc/               # Character portraits
│   ├── places/            # Location backgrounds
│   ├── scenes/           # Scene-specific media
│   └── aud/              # Audio files
├── passages/             # Game content (Twee format)
├── macros/              # Macro definitions
├── scripts/             # JavaScript files
└── styles/              # CSS stylesheets
Game State Variables:

$day / $time - Current day and time

$money - Player's money

$charm / $intellect / $fitness - Core stats

$arousal - Arousal level

$identity / $femininity / $masculinity - Gender stats

$current_activity - Current location/activity

$next - Next passage

$choice - Last choice made

$stage - Scene stage/progress

$pill_taken - Last pill taken

$npc / $npc_select - Current NPC data

$memories - Memory database

$pages - Progress tracking

$result - Minigame result

$sound - Audio toggle

==============================================
17. ADVANCED MODDING SYSTEMS
==============================================

Dynamic Content Injection:

Use ($passage_tags: "tag") for mod injection

Add passages with specific tags to inject content

Mod Configuration:

Store settings in localStorage

Use namespaced variables: $_my_mod_variable

Provide configuration passages

Cross-Mod Compatibility:

Check for existing mods before overwriting

Use unique passage names

Document dependencies

Use version checking

Performance Considerations:

Optimize image sizes (1280x720 max)

Use WebP format

Limit concurrent audio

Use (display:) for partial updates

Clean up temporary variables

==============================================
18. SCRIPTING & JAVASCRIPT INTEGRATION
==============================================

JavaScript Global Objects:

window.GE - Game engine

window.GE.updateStats() - Update UI stats

window.GE.scene_select - Scene selection

JavaScript Macros:

($get_global: "$var", "window.GE.property")

($use_global: "$var", "jsExpression", [hook])

(script: "javascript code") - Direct injection

Custom JavaScript:

Complex calculations

External API calls

Advanced UI manipulations

Browser API integration

==============================================
19. TESTING & DEBUGGING
==============================================

Debug Macros:

(debug:) - Open debug panel

(assert: condition) - Assertion checking

(assert-exists: target) - Check hook/passage

(mock-turns: n) - Mock turn count

(mock-visits: passages...) - Mock visits

Testing Tools:

Browser Developer Tools (F12)

Console logging

Variable inspection

Passage history

Save/load testing

==============================================
20. COMMUNITY & RESOURCES
==============================================

Official Channels:

Discord: https://discord.gg/B2g2YYFn2N

GitHub/GitLab: https://gitgud.io/xchange-life

Wiki: https://x-change.life/wiki/

Learning Resources:

Twine Cookbook: https://twinery.org/cookbook/

Harlowe Manual: https://twine2.neocities.org/

Twee Documentation: https://github.com/iftechfoundation/twee-spec

Tools for Modders:

Tweego: https://github.com/tmedwards/tweego

Twine 2: https://twinery.org/

Image editors: GIMP, Krita, Photoshop

Audio editors: Audacity, Ocenaudio

==============================================
21. COMPLETE WORKFLOW EXAMPLE
==============================================

Step 1: Planning

Define mod scope

Sketch passage flow

List required media

Step 2: Setup

Clone XCL repository

Create mod folder structure

Set up development environment

Step 3: Implementation

Create NPC data (if adding NPC)

Write passage content with tags

Add media files

Implement logic using XCL macros

Create integration points

Step 4: Testing

Test individual passages

Test integration

Test save/load

Test with different states

Performance testing

Step 5: Documentation

Write README

Document features

List known issues

Provide contact info

Step 6: Distribution

Package mod files

Create instructions

Share with community

Gather feedback

==============================================
22. FUTURE-PROOFING YOUR MOD
==============================================

Version Compatibility:

Check XCL version before loading

Use feature detection

Graceful degradation

Update Strategy:

Follow XCL updates

Test with new versions

Maintain changelog

Provide migration paths

Community Standards:

Follow naming conventions

Document code

Provide examples

Be responsive to bugs

Credit resources

==============================================
23. APPENDIX: QUICK REFERENCE
==============================================

Essential Macros:

Navigation: (goto:), (cs:), (screen:), (display:)

Choices: (simple_option:), (multi_option:)

Character: (is_fem:), (is_preg:), (pill:)

Memory: (remember:), (recall:), (forget:)

Media: (pic:), (vid:), (media:)

UI: (floatnote:), (updateprogress:), (icon:)

Data: (dm:), (check_dm:), (hash:)

Storage: ($get_local_storage:), ($set_local_storage:)

Common Variable Patterns:

$modname_feature = true/false - Mod toggles

$_temp_variable - Temporary calculations

$global_mod_data = (dm: ...) - Mod data storage

$npc_modname_stats = (a: ...) - NPC extension

File Naming:

passages/modname_feature.twee - Mod passages

img/mods/modname/asset.png - Mod images

scripts/modname.js - Mod JavaScript

styles/modname.css - Mod styles

==============================================
24. FINAL NOTES
==============================================

XCL is built on Twine 2 (Harlowe 3.3.3)

Story format: Harlowe with X-Lowe extensions

Primary development tool: Tweego compiler

Game is open-source under specific license

Modding is encouraged but respect original creators

Always backup your work and user saves

Test thoroughly before public release

For the latest updates, always check the official repository, wiki, and Discord.

Official Discord: https://discord.gg/B2g2YYFn2N

Happy modding!

END OF GUIDE
==============================================