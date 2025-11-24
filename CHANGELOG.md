# Change Log

## Student Record System

This version improves the original Student Record System in several key ways. The sorting logic was refactored from a manual bubble sort to the standard library `qsort` with a dedicated comparator, making sorting by marks more efficient and easier to maintain. User input handling was made more robust, especially in `modifyStudent`, where marks are now read as a line of text and validated, allowing the user to simply press Enter to keep existing marks instead of using a magic `-1` value. The initialization of the dynamic array now starts with a larger default capacity to reduce reallocations, and the main menu loop includes an additional confirmation prompt so users can exit quickly by replying `n`/`N` after any operation. Loading from file now automatically displays the loaded records so users can immediately verify the data. Throughout the code, comments and messages were clarified to better explain behavior and improve overall usability.

---

## 2025-11-23 – Docs & Presentation Prep

- **Clarified file format wording** so code and docs consistently describe the file as a plain text file with comma-separated (CSV-style) lines.
- **Documented per-choice confirmation** so the new `confirmAction` helper and menu prompts (y/n before each operation and on exit) are clearly explained in the docs.
- **Created `PRESENTATION_QA.md`** with architecture questions, beginner notes, and function-specific Q&A to help each member answer likely defence questions.
- **Updated `contributors_draft.md`** so the group leader clearly owns the core building blocks and the task table follows a realistic dependency order (leader setup → addStudent → display/search → modify/remove → stats/sort → file I/O).
- **Simplified planning docs** in `Project Documentation.md` by moving the feature→function map and edge cases into the Planning & Considerations section.
- **Added `HOW_TO_PREPARE_FOR_PRESENTATION.md`** as a step‑by‑step guide with a mini C refresher, per-role expectations, practice tips, and a note that complete beginners should read `c_tutorial.MD` first.

