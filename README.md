# 🎅 semi-secret-santa

A client-side, self-contained HTML/JavaScript utility for running Secret Santa exchanges with family exclusion rules and a "semi-secret" approach that protects the organizer's match.

## ✨ The "Semi-Secret" Advantage

This tool uses a deterministic, seeded shuffling algorithm (Mulberry32/cyrb53) entirely within a single HTML file. The results are reproducible and verifiable using the same inputs (Year, Participants, Skip Count), meaning **the organizer does not need to run or host the final draw.**

It is "semi-secret" because the organizer never sees the match, but the result is encoded in the public link's parameters. Anyone with knowledge of the code and the inputs could technically recreate the draw. For most family/friend exchanges, this provides excellent privacy while eliminating the need for complex server infrastructure.

## 🚀 Features

* **No Server Required:** Runs entirely in the browser (Vanilla JS + HTML + CSS).
* **Family Exclusions:** Easily define groups that cannot draw members from within their own group (e.g., spouses/siblings).
* **Reproducible Results:** Assignments are based on a seed (the Year), ensuring the same inputs always yield the same results.
* **Shuffle Skip Count:** Allows the organizer to quickly re-draw the pool without changing the core list, simply by adjusting the seed offset.
* **Two Distribution Modes:**
    * **Public Mode:** A single link where recipients select their name from a dropdown.
    * **Secure Tokenized Mode:** Generates a unique, tokenized link for each recipient, preventing snooping/accidental spoilers.
* **Privacy Controls:** A "Click to Reveal" button to prevent accidental spoilers, and a toggled "Parent View" to show family assignments.

## 🛠️ How to Use It (Organizer Guide)

### 1. Host the File

Upload the `index.html` file to any simple static host (e.g., GitHub Pages, Dropbox Public Folder, Google Drive Public Link) or simply run the file locally in your browser.

### 2. Enter Parameters

On the setup screen, fill out the three core inputs:

| Field | Description | Purpose |
| :--- | :--- | :--- |
| **Year (Seed)** | An integer (e.g., 2024). | The foundational seed for the entire draw. Change this to generate a completely new match set. |
| **Shuffle Skip Count** | An integer (default is `0`). | If the first valid draw is unsatisfactory, increase this number to use the 2nd, 3rd, or 4th valid draw found using the same seed. |
| **Participants & Exclusions** | Text input defining groups. | This is the source of participants and exclusion rules. Separate participants in a group with **commas** (`A, B, C`) and separate groups with **new lines** (Enter). **People on the same line cannot be matched together.** |

### 3. Choose a Link Type

| Mode | Description | Distribution |
| :--- | :--- | :--- |
| **Public Link** | Generates one universal URL. Recipients select their name from a dropdown. | Simple—paste the URL once to the whole group. Less secure, as anyone can view all names. |
| **Secure Tokenized Links** | Generates a unique URL for *every* participant (`Name: URL`). Recipients are automatically identified. | Secure—requires private distribution of each link. **Prevents accidental viewing of other people's matches.** |

## 🎁 Recipient Experience

When a recipient opens their link:

1.  They will see a welcoming message with their name (or a name selector if using Public Mode).
2.  They must **click the "Click to Reveal Your Match!"** button to see who they are buying for.
3.  If they are part of a family group, an additional **"Parent View: Show family matches"** toggle will appear, allowing them to reveal the full list of assignments within their immediate exclusion group.

## ⚙️ Technical Details (For Developers)

The core logic relies on a **seeded shuffling** process:

1.  The inputs (`Year`, `Participants`) are hashed to create a deterministic starting seed.
2.  A loop runs, attempting shuffles using offsets of the seed.
3.  Any shuffle that violates the exclusion rules is discarded.
4.  The loop finds the `(Skip Count + 1)`-th valid shuffle and locks it.
5.  The final URL contains the `Year`, the `Offset` (which determines the shuffle), and the encoded `Groups`. A secure link also includes a hashed token (`t`).

## 🤖 Attribution and AI Assistance

**The logic, core functions, and overall structure of this script were developed and refined with significant coding assistance from the Gemini model (Google).**

This repository serves primarily as documentation of a successful iterative design and debugging process, and demonstrates the author's ability to:
* Define clear functional requirements and user interfaces.
* Specify complex exclusion algorithms and cryptographic seeding methods.
* Conduct thorough debugging and refactoring of generated code (as demonstrated across multiple iterations).