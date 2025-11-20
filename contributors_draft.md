# Project Contributors — Draft (15 people)

This draft is built entirely from the functions and structure found in `student_record.c` (no external features assumed). Each contributor is shown as Person 1..Person 15. Person 1 has the hardest task to implement and Person 15 the simplest. For presentation and defence, each person is told exactly where to read in the code and given short demo and slide guidance.

Notes:
- Every assignment below maps directly to an identifiable function or closely related logic in `student_record.c` (e.g., `addStudent`, `saveToFile`).
- Each person MUST read the entire `student_record.c` file and their assigned function(s). Understanding the whole file is required for the defence.
- Every person must prepare a short presentation file for their task (suggested name: `presentation/person_<N>_task.pdf` or `.pptx`). The presentation should be one slide, one demo command, and one bullet on an edge case handled.
- Assignments were chosen so each person (or a small pair where noted) can work independently on a meaningful, testable piece of the code. Trivial global definitions (constants/types) are the group's responsibility and were established by the group leader (Person 1), not assigned as a standalone task.

---


1. Person 1 — Hardest: CLI Loop & Interaction (main)
   - Task: Own and explain the entire `main` function behavior: startup, menu loop, choice parsing, continue prompt, and final cleanup (freeing memory).
   - Read for presentation/defence: `student_record.c` — `main` function and top-of-file constants/structs.
   - Slide title: "Program Flow & CLI Orchestration". Demo: start program, show menu, choose option 2 (Display All Students) and option 11 (Exit). Explain error handling around `scanf` and `clearInputBuffer`.
   - Reviewer/Tester: Person 2

2. Person 2 — Hard: Load from File (CSV parsing)
   - Task: Implement and explain `loadFromFile` — CSV parsing, skipping header, resizing array, populating `records`.
   - Read for presentation/defence: `student_record.c` — `loadFromFile`, `initializeSystem` (for capacity behavior).
   - Slide title: "Importing Records (CSV)". Demo: prepare a sample CSV and run option 10 to load it; show parsed rows.
   - Reviewer/Tester: Person 1

3. Person 3 — Hard: Add Student (input validation + duplicates)
   - Task: Implement `addStudent` robustly: safe `fgets`, integer/float parsing loops, duplicate roll detection, dynamic resize.
   - Read for presentation/defence: `student_record.c` — `addStudent`, `initializeSystem`.
   - Slide title: "Adding Records — Validation & Safety". Demo: add a student, then try to add with same roll number to show duplicate check.
   - Reviewer/Tester: Person 4

4. Person 4 — Medium-Hard: Sorting & Comparator
   - Task: Implement `sortStudents`, `compareMarks`, and the `g_sortAscending` flag; explain qsort usage and ordering choice.
   - Read for presentation/defence: `student_record.c` — `sortStudents`, `compareMarks`, definition of `g_sortAscending`.
   - Slide title: "Sorting Student Records". Demo: add multiple students and run option 7 and 8 to show ascending/descending.
   - Reviewer/Tester: Person 3

5. Person 5 — Medium: Modify Student (optional inputs parsing)
   - Task: Implement `modifyStudent` — find by roll, accept blank input to keep current values, validate numeric input via string parsing.
   - Read for presentation/defence: `student_record.c` — `modifyStudent`.
   - Slide title: "Updating Records Safely". Demo: modify name and marks for a student; show keeping current when pressing Enter.
   - Reviewer/Tester: Person 6

6. Person 6 — Medium: Save to File (CSV export)
   - Task: Implement `saveToFile` — prompt for filename, write CSV header and rows with pass/fail status.
   - Read for presentation/defence: `student_record.c` — `saveToFile`.
   - Slide title: "Exporting Records to CSV". Demo: save to `out.csv` and open/view it to verify columns.
   - Reviewer/Tester: Person 5

7. Person 7 — Medium: Initialize System (memory management)
   - Task: Implement `initializeSystem` — set `capacity`, `count`, allocate the `students` array, and handle allocation failure.
   - Read for presentation/defence: `student_record.c` — `initializeSystem`, and `main` startup sequence.
   - Slide title: "Memory & System Initialization". Demo: explain default capacity and what happens when resizing occurs.
   - Reviewer/Tester: Person 8

8. Person 8 — Medium-Easy: Search Student
   - Task: Implement `searchStudent` — locate by roll number and print full details including PASS/FAIL.
   - Read for presentation/defence: `student_record.c` — `searchStudent`.
   - Slide title: "Lookup: Search by Roll Number". Demo: search for an existing and a non-existing roll.
   - Reviewer/Tester: Person 7

9. Person 9 — Medium-Easy: Calculate Average Marks
   - Task: Implement `calculateAverageMarks` — compute and print total student count and average.
   - Read for presentation/defence: `student_record.c` — `calculateAverageMarks`.
   - Slide title: "Class Average". Demo: show average before and after adding a student with high/low marks.
   - Reviewer/Tester: Person 10

10. Person 10 — Easy: Remove Student
    - Task: Implement `removeStudent` — remove by roll number and shift array entries down.
    - Read for presentation/defence: `student_record.c` — `removeStudent`.
    - Slide title: "Removing Records". Demo: remove a student and show the updated list.
   - Reviewer/Tester: Person 9

11. Person 11 — Easy: Display All Students
    - Task: Implement `displayAllStudents` — format rows, compute PASS/FAIL for display, print table header.
    - Read for presentation/defence: `student_record.c` — `displayAllStudents`.
    - Slide title: "Displaying Records". Demo: add two students and run option 2 to show the table.
   - Reviewer/Tester: Person 12

12. Person 12 — Very Easy: Menu Display
    - Task: Implement `displayMenu` — print numbered options exactly as they'll be used in the CLI demo.
    - Read for presentation/defence: `student_record.c` — `displayMenu`.
    - Slide title: "User Menu". Demo: start program and show the menu screen.
   - Reviewer/Tester: Person 11

13. Person 13 — Very Easy: Welcome / Banner
    - Task: Implement `displayWelcomeMessage` — concise banner at program start.
    - Read for presentation/defence: `student_record.c` — `displayWelcomeMessage`.
    - Slide title: "Program Banner & UX". Demo: show banner when starting the program.
   - Reviewer/Tester: Person 14

14. Person 14 — Simplest: Input Cleanup / Buffer
   - Task: Implement `clearInputBuffer` — consume trailing input so `fgets` and `scanf` interplay is safe.
   - Read for presentation/defence: `student_record.c` — `clearInputBuffer` and mention where it's called.
   - Slide title: "Handling stdin Cleanly". Demo: show that invalid `scanf` input doesn't break subsequent `fgets`.
   - Reviewer/Tester: Person 13

15. Person 15 — Feasible & Useful: Sample Data & Presentation Coordination
   - Task: Create and maintain `samples/students_sample.csv` and collect/standardize each person's single-slide presentation into `presentation/` (ensure naming and format). Do *not* create an automated runner script — keeping the demo manual and explicit is in-scope; Person 15 should provide clear demo steps instead (commands and filenames) so presenters can run them reliably.
   - Read for presentation/defence: `student_record.c` — top of file (to understand the data model) and `loadFromFile`/`saveToFile` to ensure the sample CSV matches parsing/writing format.
   - Slide title: "Sample Data & Presentation Coordination". Demo: open `samples/students_sample.csv`, run option 10 to load it, then option 2 to display, option 7 to sort, option 9 to save to `out.csv`, and option 11 to exit. This reduces typing during the defence and ensures everyone can demo reliably.

---


How to read and present (shared guidance for everyone):
- Primary source: `student_record.c` — each person should open and read the entire file and then focus on their assigned function(s). Use the function name search to jump straight to the implementation.
- Slides: each person prepares 1 slide covering: what the function does, why key design choices were made (validation, error handling, resizing), one edge case handled, and a 10–20 second demo command. Save your slide as `presentation/person_<N>_task.pdf` or `.pptx`.
-- Demo commands: run the binary (compiled from `student_record.c`) and use the menu option number for your function (e.g., option 5 to remove, option 10 to load CSV). Test your demo using `samples/students_sample.csv` (Person 15 will coordinate this file and provide explicit demo steps).

How we all worked together — single-paragraph summary for the defence:
- The team followed a modular, function-first approach: the group leader set the data model and main flow, and each team member implemented a single function from the `student_record.c` file. We performed small, focused code reviews in pairs (one implementer + one reviewer) to check input validation and memory handling. After individual implementations, we integrated changes into the single `student_record.c` program, ran manual integration tests (sample CSVs and interactive CLI flows), fixed edge cases (bad input, duplicate roll numbers, file I/O errors), and created this contributors draft mapping each person to the function they implemented.

For the presentation every person will prepare a single-slide presentation file and a 15–30s demo. During defence each person will:
- Briefly state the function they own and why the approach is safe/appropriate (10–20s).
- Run their pre-seeded demo command (10–20s).
- Answer one question about edge cases handled.

This structure ensures each person both owns a concrete, testable piece of code and is prepared to defend it, while Person 15 provides the reproducible demo flow so the whole team can present smoothly.

Quick checklist for the presentation:
- Each person: open `student_record.c`, read the entire file and your assigned function, and prepare one slide plus a 15–30s demo. Save your slide as `presentation/person_<N>_task.pdf` or `.pptx`.
- Group lead (Person 1) will introduce the architecture and run the integrated demo.
-- Person 15: create `samples/students_sample.csv` and provide clear demo steps so demos are pre-seeded and reliable; everyone must test their demo with the sample data at least once before the defence.

Reviewer/tester pairing and oversight:
- Each implementer is paired with one reviewer/tester listed under their assignment (the reviewer is responsible for performing a focused code review, writing or running one simple test that exercises the implemented behavior, and validating the presenter's demo before the rehearsal).
- Pairing summary: (Person 1 <-> Person 2), (Person 3 <-> Person 4), (Person 5 <-> Person 6), (Person 7 <-> Person 8), (Person 9 <-> Person 10), (Person 11 <-> Person 12), (Person 13 <-> Person 14), and Person 15 coordinates the sample data and presentation collection and is additionally reviewed by Person 1 for readiness.
- The group leader (Person 1) will perform a final check across all implementations and demos to ensure consistency, safety (input validation and memory), and readiness for defence.

If you want, I can now:
- Insert TODO comment headers inside `student_record.c` above each function to make it explicit which person owns which function.
- Create a small `samples/students_sample.csv` and a `run_demo.sh` script that exercises a short demo sequence for the defence.

Tell me whether to add TODO headers or generate the sample CSV + run script next, and I'll apply those changes.
