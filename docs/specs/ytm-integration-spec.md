# YTM Player Interactive Widget Spec

## Goal
Build a persistent, floating "Now Playing" terminal widget that serves as an interactive tribute to `peternaame-boop/ytm-player`, playing local audio while explicitly crediting the original creator.

## User Story
"As a portfolio visitor, I want to use a `ytm` command to spawn a background music player styled like the real ytm-player TUI, so that I can experience the developer's aesthetic taste and discover a cool open-source tool without assuming the developer built it."

## Improvements Added
1. **Animated ASCII Equalizer:** A dynamic visual component (e.g., `▂ ▃ ▄ ▅`) that animates while audio is playing to simulate a real TUI feel.
2. **Explicit Credit Header:** The top border of the widget will explicitly say `[Tribute: peternaame-boop/ytm-player]`, making it clear you are showcasing a tool you admire, not one you made. Clicking this link opens their GitHub repo.
3. **Sticky UI / Background Playback:** When you type `ytm`, the widget anchors to the bottom right of the screen so users can keep typing commands while listening to the music.
4. **Playback Controls:** Fully functional `[Pause]`, `[Play]`, and `[Close]` (or X) ASCII buttons that control the HTML5 `<audio>` element playing `reel_audio.mp3`.
5. **Draggable (Bonus):** If possible, making the ASCII window draggable around the viewport.

## Non-Goals
- We are NOT claiming authorship of `ytm-player`.
- We are NOT integrating a live YouTube Music/Spotify API right now (we will use the local `reel_audio.mp3` as a curated portfolio track to ensure it always works without API rate limits or auth).
- We are NOT compiling or running the real Go binary in the browser.

## API/Interface
- **Command:** `ytm` -> Toggles the widget visibility. `ytm --help` -> Shows details about the command and credits.
- **DOM Structure:**
  ```html
  <div id="ytm-widget" class="tui-window draggable">
     <div class="tui-header">Tribute: [peternaame-boop/ytm-player] <span class="close-btn">[X]</span></div>
     <div class="tui-body">
       <div class="track-info">Playing: My Curated Portfolio Mix</div>
       <div class="ascii-equalizer">▃ ▅ █ ▇ ▅ ▃</div>
       <div class="controls">[< Prev] [|| Pause] [Next >]</div>
     </div>
     <audio id="ytm-audio-player" src="reel_audio.mp3" loop></audio>
  </div>
  ```
- **Styling:** We will heavily reuse your existing ASCII ANSI color variables (`--brand-gold`, `--cyan`, `--green`) and the `window` CSS classes to make it fit seamlessly into the terminal ecosystem.

## Edge Cases (What could go wrong?)
1. **Autoplay Policies:** Browsers block audio autoplay until the user interacts with the page.
   *Mitigation:* The user typing the command `ytm` counts as interaction, so audio playback will be allowed when the widget pops up.
2. **Mobile Screen Clutter:** A floating widget might block the terminal input on small phone screens.
   *Mitigation:* Give it a fixed small height on mobile (`bottom: 10px`), or make it auto-minimize to just a `[Playing 🎵]` badge when space is tight.
3. **Multiple Audio Streams:** If they spawn `ytm` while another audio source in the terminal is playing.
   *Mitigation:* Ensure any existing global audio in the terminal is paused when `ytm` starts.
