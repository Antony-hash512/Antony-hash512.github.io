# Development Diary: YouTube Previewer

Goal: Create a local tool to preview YouTube video previews (titles, thumbnails, channel names) for the home page and sidebar. Previews should look exactly like on real YouTube: scaled down and with a timecode overlay in the bottom right corner. Long titles must be truncated exactly like on real YouTube (at the right position and with an ellipsis at the end).

The following development stages outline the challenges we (AI assistant and human) faced and how we solved them.

## Stage 1: Start and Basic Functionality (T + 00:00 — 00:10), where T is the first commit
**Objective:**
Create a local tool for previewing YouTube video previews (titles, thumbnails, channel names) for the home page and sidebar.
Added the ability to load local images via standard file selection ("Open" button) and Drag-and-Drop from the operating system into the browser window.

## Stage 2: Styling and Initial Hurdles (T + 00:54 — 01:21)
**Objective:**
Improve the visual appearance to match real YouTube exactly.

**Bugs/Challenges:**
- Standard fonts didn't look like YouTube's.
- Titles were merging, font-weight was too heavy, and spacing was off (sidebar content was hitting the edges). This is critical as the tool's purpose is to show exactly how it looks on real YouTube without uploading to avoid spam filters/shadow bans.
- A typo in the default channel name ("Hacker Lifestyle" instead of "Hacker's LifeStyle").

**Solutions/Workarounds:**
- To achieve accurate rendering, the AI agent used a browser to inspect real YouTube code and extract CSS rules and fonts.
- Integrated `Google Fonts (Roboto)` for an authentic look.
- Applied a quick `hotfix`: forced lower `font-weight` for home and sidebar titles, added `padding-right` for sidebar titles.
- Fixed the default channel name.
- Implemented exact title truncation logic as seen on real YouTube.

## Stage 3: Time Display Refinement (T + 01:41 — 01:55)
**Objective:**
Improve video duration badges (add hours* support).

\* hours, in addition to minutes and seconds.

**What was done:**
Initially added hours (HH:MM:SS) to the sidebar and updated placeholders. Later realized this isn't always needed, so implemented a toggle option to show/hide hours in badges, switching between HH:MM:SS and MM:SS formats.

## Stage 4: Scaling to Multi-Slots (T + 34:56 — 35:05)
**Objective:**
Move from single input to a three-slot system for side-by-side comparisons.

**Bugs/Challenges:**
- Adding three slots caused the layout to break. Elements weren't aligning correctly, and borders/padding were applied inconsistently.

**Solutions/Workarounds:**
- The human operator provided a specific commit ID where the layout wasn't broken to restore the base while keeping the new three-slot functionality.
- **Structural Refactoring:** Wrapped sidebar and home page elements in new parent `div` containers. This allowed for correct `border` and `padding` application.
- **Layout Change:** Switched home page elements' `flex-direction` from `column` to a `wrapping flex row` for a card-like grid layout.
- Created separate Drag-and-Drop and file upload areas for each slot. Added styles to these areas for better visibility and "Open" buttons as an alternative to Drag-and-Drop.

## Stage 5: UX Improvements and Cleanup (T + 35:21 — 35:42)
**Objective:**
Improve user convenience (text copying) and clean up the code.

**Bugs/Challenges:**
- Extraneous backticks appeared in the HTML, likely artifacts from LLM code generation.

**Solutions/Workarounds:**
- Identified and removed unnecessary backticks (`fix: remove extraneous backticks`).
- Implemented convenient Copy & Paste functionality for title inputs using buttons and shortcuts to avoid manual retyping.
- Synchronized default view counts across all three slots.

## Stage 6: Final Polish and Display Settings (T + 36:43 — 38:07)
**Objective:**
Give users more control over display and document the project.

**What was done:**
- Added a checkbox to toggle the home page layout between "row" and "column".
- Added color coding for each slot to improve navigation.
- Wrote and updated `README.md`, explicitly instructing users to open the file via browser. Added GPLv3 license.

---

# Development Diary: Portfolio Website

In addition to the tool itself, we developed a portfolio website to showcase the projects. 
The timeline (T') starts from the first commit in the `Antony-hash512.github.io` repository.

## Stage 1: Creating the Website Foundation (T' + 00:03 — 00:11)
**Objective:**
Create a modern and stylish landing page (`index.html`) to present the developer and their tech stack.

**What was done:**
- Migrated the `YouTube Previewer` project files to the root of the repository.
- Designed a premium "Glassmorphism" interface featuring a dark theme, gradient background elements (blobs), and blur effects.
- Imported tech stack details (Arch Linux, Rust, Godot, etc.) from `README.aboutme.md`.
- Integrated technology icon badges via `skillicons.dev`.

## Stage 2: Project Integration via HTML Wrappers (T' + 00:13 — 00:23)
**Objective:**
Create beautiful pages for documentation (`youtube-preview.md`) and the development diary (`dev_diary.md`) so they look like integrated parts of the website rather than raw text files on GitHub.

**What was done:**
- Decided to use the `marked.js` library for dynamic Markdown rendering directly in the browser.
- Created `youtube-preview.html` and `dev_diary.html`. These pages fetch the `.md` files on the fly and apply the website's styling.
- Added "Back to Home" buttons and links to the raw file versions, maintaining Markdown as the single source of truth.

## Stage 3: Multilingual Support and Deep Linking (T' + 00:33 — 00:45)
**Objective:**
Make the development diary accessible to an English-speaking audience and allow sharing direct links to a specific language.

**What was done:**
- The AI assistant translated the diary text into English (`dev_diary_en.md`).
- Added a minimalist **RU / EN** language switcher to the `dev_diary.html` page.
- Implemented URL parameter reading (`?lang=en` or `?lang=ru`) using JavaScript (`URLSearchParams`).
- Configured automatic URL updates in the browser address bar without page reloads (via `history.pushState`) when the language switcher is clicked.

## Stage 4: Multimedia Integration (T' + 02:30 — 02:35)
**Objective:**
Embed a demonstration YouTube video into the development diary, adding the ability to expand/collapse it.

**Bugs/Challenges:**
- Initially, the video was hidden and didn't display upon page load because the `src` attribute in the `iframe` was empty (an attempt at "lazy" loading).

**Solutions/Workarounds:**
- Created a container with smooth CSS animations (`max-height` and `opacity`) for a collapsible effect.
- Set the `expanded` and `active` classes by default in the HTML and immediately defined the `src` link so the player was ready from the first second.
- Added synchronization for the button text ("Show Video" / "Hide Video") based on the current language immediately upon page load.
