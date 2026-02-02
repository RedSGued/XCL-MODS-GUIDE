==============================================
X-CHANGE LIFE (XCL) COMPLETE MODDING GUIDE
==============================================

Last Updated: February 2024
Game Version: X-Lowe
Repository: https://gitgud.io/xchange-life/xcl-twee-xlowe
Official Wiki: https://x-change.life/wiki/

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
9. References & Links

==============================================
1. NAVIGATION MACROS
==============================================
These macros control movement between passages and screen layouts.

(goto: "passage") - Immediate navigation to another passage
(screen: "right", "center") - Three-panel layout with right sidebar
(cs: target, mode) - Change-screen navigation (supports "next", "goto", "nostop")
(gt: "passage") - XCL wrapper for goto with cleanup
(display: "passage") - Render another passage in-place

Quick navigation wrappers:
($cs: "passage") - Convenience wrapper for (cs:)
($nx: "passage") - Fullscreen navigation wrapper
($screen: "right", "center") - Deprecated, use (screen:) directly
($smart_screen: "sidebar", "media", "text", "options") - Smart layout builder

==============================================
2. CHOICE SYSTEMS
==============================================
Create interactive choices for players.

(simple_option: target, labels..., flags) - Single option group with flags
(multi_option: option_arrays...) - Multiple option groups at once
($simple_option: target, labels...) - Alias of simple_option
($multi_option: options...) - Wrapper for multi_option
($parse_option: target, labels...) - Low-level option builder

Flags for options:
- "display" - Show as display (no navigation)
- "goto" - Full goto navigation
- "next" - Next-screen flow
- "nostop" - Refresh without stopping sounds
- false - Hide the option block

Example:
(simple_option: "buy flowers", "display", "Daisies", "Roses", "Lilies")
(multi_option: (a:"buy flowers","Daisies","Roses"), (a:"day","goto","Leave"))

==============================================
3. CHARACTER & NPC SYSTEMS
==============================================

Character Status Macros:
(is_fem:) - Returns true if female
(is_male:) - Returns true if male
(is_preg:) - Returns true if pregnant
(knows_preg:) - Returns true if pregnancy is known
(is_bim:) - Returns true if has "bimbo" effect
(is_pp:) - Returns true if has "people pleaser" effect
(pill: "name") - Checks if specific pill was taken

NPC Handling:
($show_base_npc: "npc_id") - Load base NPC into UI
($char_passage: base, core, fallback) - Character-specific content
($bargirl_passage: base, core, fallback) - NPC-specific content
($passage_tags: "tag") - Display all passages with tag (mod injection)

Modding Characters:
Create passages named "base_passage character_id" for character-specific content.
Example: "sex transactional kiss alexia" for Alexia-specific content.

Wardrobe System:
(outfitdb: "character", outfit_ids) - Get outfits for character
(getoutfit: "outfit_key") - Get outfit details

==============================================
4. MEMORY SYSTEM
==============================================

Memory Structure:
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
(recall: id, tag_checks...) - Recall memory by id
(forget: id) or (forget:) - Delete memory/prune expired
(memory_time: memory) - Natural language time phrase
(days_since: "id") - Days since memory was created
(show_memory: memory, options) - Render memory media

Example:
(remember: "stepsis shoplifting", 14, "stepsis", "none", "caught", "shoplifting")
(recall: "stepsis shoplifting", "not contains", "forgotten")

Auto-added tags: "aroused", "drunk" based on character state.
Location set based on $current_activity.

==============================================
5. ACTIVITIES & GAMEPLAY
==============================================

Activities System:
- Time-based progression
- Location-specific (Mall, Gym, Bar, Home)
- Stat modifications
- Memory creation

Gameplay Macros:
($set_mood: "mood", "reason") - Set character mood
($set_status: "status", "reason") - Add status effect
($register_orgasm:) - Log orgasm event
($gain_money: amount) - Add money with UI updates
($pay_money: amount) - Subtract money
($gain_arousal: amount) - Gain arousal with styling
($core_female_status:) - Returns "bimbo" or "female"

Stat Checks:
($willpower_check: "passage", difficulty)
($charm_check: "passage", difficulty)
($intellect_check: "passage", difficulty)
($fitness_check: "passage", difficulty)

Compulsions System:
- Automatic behavior triggers
- Tied to side effects (Bimbo, People Pleaser)
- Can override player choices

Dreams System:
- Add [dream] tag to passages
- Add to $overnight_messages array
- Use memory system for persistence
- Example:
  :: Dream Passage [dream]
  (remember: "dream_id", 7, "none", "none", "dream", content)

==============================================
6. MEDIA & UI MACROS
==============================================

Media Display:
(pic: image, format, class, options) - Display image
(vid: video, format, class, options) - Display video
(media: path_or_html, options) - Smart media (auto-detect type)
($pic: image, format, class) - Legacy image wrapper
($vid: video, format, class) - Legacy video wrapper

Format options: "left", "right", "center", "left squared", etc.
Options datamap: (dm: "width", "75%", "aspect-ratio", "16/9", "filter", "sepia(30%)")

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
(show_tooltip_text: text, tip) - Text-only version
(cleartooltips:) - Clear all tooltips
(tooltipgc:) - Garbage collect tooltips
(refreshtooltips:) - Reposition tooltips

Text Styling:
($centered: [content]) - Centered options block
($highlight: [content]) - Highlight span
($shadow: [content]) - Shadow text
($bimbo: [content]) - Bimbo-styled narration
($bimbo_dialogue: [content]) - Bimbo dialogue with quotes
($heading: "text") - Large heading
($caps: "text") - Capitalize each word

Notifications:
($notification: "message") - Temporary animated
($notification_still: "message") - Persistent

==============================================
7. DATA & UTILITIES
==============================================

Randomness:
(newseed:) - Generate fresh random seed
(twist: min, max) - Random integer
(twirl: values...) - Random value from arguments
(twerk: hooks...) - Random code hook execution
(twisted: values...) - Shuffle array/values
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
(hash: value) - Generate deterministic hash
(indexof: array, item) - Find item index
(intersection: a, b) - Array intersection
(remove: array, item, count) - Remove from array

Storage:
($get_session_storage: key, default) - Read sessionStorage
($get_local_storage: key, default) - Read localStorage
($set_session_storage: key, value) - Write sessionStorage
($set_local_storage: key, value) - Write localStorage
(savegameto: "slot") - Save game state
(get_storage: "type", key, default) - Generic storage
(set_storage: "type", key, value) - Generic storage write

Audio:
($play: type, track, delay) - Play audio tracks
Types: "sound", "ambience", "sex loop", "song", "scene", "story"

Debug:
(clearstandardvars:) - Reset engine variables
(del: "$var1", "$var2") - Delete variables
($delete_global_variable: "$var") - Reset global variable
($get_global: "$var", "js_expr") - Copy JS to Twine variable
($use_global: "$var", "js_expr", hook) - Temporary JS value
($record_timing: "point") - Performance profiling
(win:) - Check if $result is "pass"

==============================================
8. MODDING TOOLS
==============================================

Essential Modding Macros:
($char_passage: base, core, fallback) - Character-specific content handler
($bargirl_passage: base, core, fallback) - NPC-specific content handler
($passage_tags: "tag") - Mod injection points
($show_base_npc: "npc_id") - Load NPC for mods/debug

Best Practices:
1. Use official macros for compatibility
2. Prefix custom variables: $_modname_variable
3. Test with base game updates
4. Use (display:) for UI refreshes
5. Clean up unused variables
6. Document mod requirements

Creating Activities:
1. Define in location system
2. Create passage sequence
3. Add stat changes
4. Optionally create memories
5. Integrate with navigation

Adding NPCs:
1. Add NPC data to game databases
2. Create character-specific passages
3. Use char_passage/bargirl_passage macros
4. Add portraits to img/npc/ folder

Adding Dreams:
1. Create passage with [dream] tag
2. Add to $overnight_messages
3. Use memory system for persistence
4. Consider dream-specific visuals

==============================================
9. REFERENCES & LINKS
==============================================

Official Documentation:
• Bar Girl System: https://x-change.life/wiki/docs/bar-girl/
• Playable Character: https://x-change.life/wiki/docs/playable-character/
• Activities System: https://x-change.life/wiki/docs/quick-guide-to-the-activities-system/
• Custom Macros: https://x-change.life/wiki/docs/custom-x-change-life-macros-you-can-use-in-mods/
• Creating Mods: https://x-change.life/wiki/docs/creating-mods/
• Compulsions: https://x-change.life/wiki/docs/compulsions/
• Memory System: https://x-change.life/wiki/docs/xcl-memory-system/
• Adding Dreams: https://x-change.life/wiki/docs/adding-a-dream/

Code Repository:
• https://gitgud.io/xchange-life/xcl-twee-xlowe

Harlowe Macros (Standard):
• Variables: (set:), (put:), (move:)
• Control Flow: (if:), (unless:), (else:), (for:), (loop:)
• Data Structures: (a:), (dm:), (ds:), (range:), (find:)
• Strings: (str:), (substring:), (uppercase:), (lowercase:)
• Math: (abs:), (min:), (max:), (pow:), (sqrt:)

TGP Macros (Also Available):
• (indexofpage:), (indexofstatchange:), (and:), (not:)
• (choice:), (resettopage:), (change_stat:), (read_stat:)
• (notification:), (audiotoggle:), (playwhenread:), (map:)

==============================================
10. GAME ARCHITECTURE & STRUCTURE
==============================================

Game Folder Structure:
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
$day / $time              - Current day and time
$money                    - Player's money
$charm / $intellect / $fitness - Core stats
$arousal                  - Arousal level
$identity / $femininity / $masculinity - Gender stats
$current_activity         - Current location/activity
$next                     - Next passage to navigate to
$choice                   - Last choice made
$stage                    - Scene stage/progress
$pill_taken               - Last pill taken
$npc / $npc_select        - Current NPC data
$memories                 - Memory database array
$pages                    - Game progress tracking
$result                   - Minigame result ("pass"/"fail")
$sound                    - Audio toggle state

Passage Tags System:
[fullscreen]    - Passage uses fullscreen mode (cs: behaves like "next")
[goto]          - Passage uses immediate goto (cs: behaves like (goto:))
[dream]         - Dream passage (added to $overnight_messages)
[storylet]      - Harlowe storylet system (if used)
[exclusivity:X] - Storylet exclusivity
[urgency:X]     - Storylet urgency

Time System:
- Day counter increments after sleep
- Time divided into segments: Morning, Afternoon, Evening, Night
- Activities consume time segments
- Automatic time progression in certain scenes

Economy System:
- Money earned through activities/jobs
- Expenses for items, outfits, services
- Dynamic pricing based on locations
- (currency: amount) macro for formatting

==============================================
11. ADVANCED MODDING SYSTEMS
==============================================

Dynamic Content Injection:
- Use ($passage_tags: "tag") for mod injection points
- Add passages with specific tags to inject content
- Example: Add passage tagged "bar_options" to inject new bar options

Mod Configuration:
- Store mod settings in localStorage: ($set_local_storage: "modname_setting", value)
- Use namespaced variables: $_my_mod_variable
- Provide configuration passages if needed

Cross-Mod Compatibility:
1. Check for existing mods before overwriting content
2. Use unique passage names: "mymod_feature" not "feature"
3. Document dependencies and conflicts
4. Use version checking in mod initialization

Save Game Integration:
- Mod data should be included in save games
- Use standard variables or custom storage with save/load hooks
- Consider migration paths for mod updates

Performance Considerations:
- Optimize image sizes (recommend 1280x720 max for scenes)
- Use WebP format for better compression
- Limit concurrent audio tracks
- Use (display:) for partial updates instead of full navigation
- Clean up temporary variables

==============================================
12. SCRIPTING & JAVASCRIPT INTEGRATION
==============================================

JavaScript Global Objects:
window.GE                 - Game engine object (if available)
window.GE.updateStats()   - Update UI stats
window.GE.scene_select    - Scene selection data

JavaScript Integration Macros:
($get_global: "$var", "window.GE.property")
($use_global: "$var", "jsExpression", [hook])
(script: "javascript code")  - Inject JavaScript directly

Custom JavaScript Functions:
Add to Story JavaScript or use (script:) macro for:
- Complex calculations
- External API calls (if allowed)
- Advanced UI manipulations
- Integration with browser APIs

Example: Custom Achievement System
(script: "window.myModAchievements = window.myModAchievements || {};")
(if: (check_dm: window.myModAchievements, "achievement1", "is", true))[...]

==============================================
13. TESTING & DEBUGGING
==============================================

Debug Macros:
(debug:)                    - Open debug panel
(assert: condition)         - Assertion checking
(assert-exists: target)     - Check hook/passage exists
(mock-turns: n)            - Mock turn count (debug mode)
(mock-visits: passages...) - Mock passage visits

Testing Tools:
1. Use browser Developer Tools (F12)
2. Console logging: (script: "console.log('test', $variable);")
3. Variable inspection in debug panel
4. Passage history: (history:), (visited: "passage")
5. Save/load testing

Common Issues & Solutions:
1. Macros not working: Check spelling and parameter count
2. Images not showing: Verify path and file extension
3. Variables not persisting: Ensure using $ not _ prefix for story variables
4. Navigation loops: Check passage tags and cs: modes
5. Performance issues: Optimize media, reduce passage complexity

==============================================
14. COMMUNITY & RESOURCES
==============================================

Official Channels:
• Discord: [XCL Discord Server] (search community)
• GitHub/GitLab: https://gitgud.io/xchange-life
• Wiki: https://x-change.life/wiki/

Learning Resources:
• Twine Cookbook: https://twinery.org/cookbook/
• Harlowe Manual: https://twine2.neocities.org/
• Twee Documentation: https://github.com/iftechfoundation/twee-spec

Tools for Modders:
• Tweego: Twee compiler (https://github.com/tmedwards/tweego)
• Twine 2: Visual editor (https://twinery.org/)
• Image editors: GIMP, Krita, Photoshop
• Audio editors: Audacity, Ocenaudio
• JSON validators for data files

Sample Mod Structure:
my-awesome-mod/
├── README.md
├── manifest.json          # Mod metadata
├── passages/              # Additional passages
├── img/                   # Custom images
│   └── mymod/
├── scripts/              # JavaScript additions
└── styles/               # CSS overrides

Mod Manifest Example:
{
  "name": "My Awesome Mod",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "Adds new features to XCL",
  "requires": "XCL v2.0+",
  "conflicts": ["other-mod"],
  "files": ["passages/mymod.twee", "img/mymod/"]
}

==============================================
15. COMPLETE WORKFLOW EXAMPLE
==============================================

Step 1: Planning
- Define mod scope: New NPC, activity, or system
- Sketch passage flow
- List required media assets

Step 2: Setup
- Clone XCL repository or extract game files
- Create mod folder structure
- Set up development environment

Step 3: Implementation
1. Create NPC data structure (if adding NPC)
2. Write passage content with appropriate tags
3. Add media files to correct folders
4. Implement custom logic using XCL macros
5. Create integration points with base game

Step 4: Testing
1. Test individual passages
2. Test integration with base systems
3. Test save/load functionality
4. Test with different character states
5. Performance testing

Step 5: Documentation
- Write README with installation instructions
- Document new features
- List known issues or limitations
- Provide contact information

Step 6: Distribution
- Package mod files
- Create installation instructions
- Share with community
- Gather feedback and iterate

==============================================
16. FUTURE-PROOFING YOUR MOD
==============================================

Version Compatibility:
- Check XCL version before loading mod
- Use feature detection instead of version checking
- Graceful degradation for missing features

Update Strategy:
- Follow XCL update announcements
- Test mod with new versions promptly
- Maintain changelog for your mod
- Provide migration paths for users

Community Standards:
- Follow established naming conventions
- Document your code
- Provide examples of usage
- Be responsive to bug reports
- Credit resources and inspiration

==============================================
APPENDIX: QUICK REFERENCE
==============================================

Essential Macros Quick Reference:
Navigation: (goto:), (cs:), (screen:), (display:)
Choices: (simple_option:), (multi_option:)
Character: (is_fem:), (is_preg:), (pill:)
Memory: (remember:), (recall:), (forget:)
Media: (pic:), (vid:), (media:)
UI: (floatnote:), (updateprogress:), (icon:)
Data: (dm:), (check_dm:), (hash:)
Storage: ($get_local_storage:), ($set_local_storage:)

Common Variable Patterns:
$modname_feature = true/false        - Mod feature toggles
$_temp_variable                      - Temporary calculations
$global_mod_data = (dm: ...)         - Mod data storage
$npc_modname_stats = (a: ...)        - NPC extension data

File Naming Conventions:
passages/modname_feature.twee        - Mod passages
img/mods/modname/asset.png           - Mod images
scripts/modname.js                   - Mod JavaScript
styles/modname.css                   - Mod styles

==============================================
FINAL NOTES
==============================================

• XCL is built on Twine 2 (Harlowe 3.3.3)
• Story format: Harlowe with X-Lowe extensions
• Primary development tool: Tweego compiler
• Game is open-source under specific license
• Modding is encouraged but respect original creators
• Always backup your work and user saves
• Test thoroughly before public release

For the latest updates, always check the official repository and wiki.

Happy modding!

==============================================
ADDITIONAL NOTES
==============================================
• XCL uses Twine with Harlowe story format
• Custom macros extend functionality
• Mods should be compatible with base game updates
• Community resources available on Discord/forums
• Always backup before modifying game files

For questions: Check Discord community or official forums.

END OF GUIDE
==============================================
