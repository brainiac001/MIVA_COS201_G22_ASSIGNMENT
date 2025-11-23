# Project Contributors — Team Task Assignments & Presentation Prep

## Team Overview
This project is a Student Record System in C. The team is divided into 15 roles. Each pair of persons implements the same function; the best version is selected. All persons review the group leader's work to understand the architecture. Two shared reviewers per task pair ensure quality.

---

## Task Assignments

| Person | Task | Function | Reviewers |
|--------|------|----------|-----------|
| **1** | **Group Leader ** | Architecture and core building blocks: `main` (CLI orchestration, menu loop, confirmation prompts, memory cleanup, consume trailing stdin, allocate array), data model structs (`Student`, `StudentRecordSystem`), `displayMenu`, `displayWelcomeMessage`, `clearInputBuffer`, `initializeSystem`, `confirmAction` | Everyone reviews |
| 2, 3 | Add Student | `addStudent` (input validation, duplicate detection, resize) | Persons 4, 5 |
| 4, 5 | Display All Students | `displayAllStudents` (format table, compute PASS/FAIL) | Persons 6, 7 |
| 6, 7 | Search Student | `searchStudent` (locate, display, not-found handling) | Persons 8, 9 |
| 8, 9 | Modify Student | `modifyStudent` (find, partial updates, preserve blanks) | Persons 10, 11 |
| 10, 11 | Remove Student | `removeStudent` (locate, shift array, decrement count) | Persons 12, 2 |
| 12, 2* | Calculate Average | `calculateAverageMarks` (sum, average, display) | Persons 3*, 4* |
| 3*, 4* | Sort Students | `sortStudents`, `compareMarks` (qsort, ascending/descending) | Persons 5*, 6* |
| 5*, 6* | Save to File | `saveToFile` (CSV export, headers, pass/fail status) | Persons 8*, 7* |
| 7*, 8* | Load from File | `loadFromFile` (CSV parsing, array resize, data population) | Persons 9*, 10* |

*Alt task (fallback if primary task completed early)

---

## Team Process

We organised the work in **phases**, so that functions which are more independent were designed and implemented first, and functions that depend on others came later.

1. **Phase 0 – Leader skeleton and core utilities**  
   - Person 1 (leader) defines the data structures and core utilities that everything else depends on: `Student`, `StudentRecordSystem`, `initializeSystem`, `main`, `displayMenu`, `displayWelcomeMessage`, `clearInputBuffer`, and `confirmAction`.

2. **Phase 1 – Core data creation**  
     - `addStudent` (validated inserts, duplicate check, capacity checks).  

3. **Phase 2 – Read‑only features on created data**  
   - Once `addStudent` exists and we can store records, we implement the functions that **read** and present those records:
     - `displayAllStudents` (table formatting and PASS/FAIL display).  
     - `searchStudent` (find by roll number).  

4. **Phase 3 – Write operations**  
   - Functions that **change** the in‑memory array are implemented next:  
     - `modifyStudent` (editing an existing record).  
     - `removeStudent` (shifting and shrinking the logical count).  

4. **Phase 3 – Calculations and sorting**  
   - Once we can add, view, and remove students, we implement:  
     - `calculateAverageMarks` (sum and average).  
     - `sortStudents`/`compareMarks` (ascending/descending order using `qsort`).  

5. **Phase 4 – File operations**  
   - Finally, we add functions that depend on a stable in‑memory representation but extend it with storage:  
     - `saveToFile` (writing comma‑separated lines to a text file).  
     - `loadFromFile` (rebuilding the in‑memory list from the file).  


---

## Final Documentation Structure

1. **Table of Contents** — all sections with page numbers
2. **Group Details** — group number, member names, student ID
3. **Requirements** — original problem statement
4. **Planning and Considerations** — modular approach, data model, code review strategy
5. **Program Flow and User Story** — Program Flow diagram, how the program runs(User Story),
6. **Program/Code** — full `student_record.c` code
7. **Quick Code Explanation** — 1-2 paragraphs per function: what it does, design choices, edge case handled
8. **Conclusion** 

---

## Preparation Instructions

### For All Team Members:
1. **Read** `c_tutorial.MD` (if new to C), then `student_record_commented.c` (learn structure).
2. **Study** `student_record.c` carefully.
3. **For your assigned task**: Read both implementations (yours and your pair's), understand the differences.
4. **Prepare ONE slide** covering:
   - Function name and what it does
   - Why the design choices were made (safety, efficiency, user experience)
   - One edge case it handles well
   - A short demo command (e.g., "I will run option 5 to remove a student")
5. **Test your demo**: Compile with `gcc -o student_record student_record.c`, run your function, time it (10–30s).
6. **Save your slide** as `presentation/person_<N>_task.pdf` or `.pptx`.
  Read `HOW_TO_PREPARE_FOR_PRESENTATION.md` for more details on how to prepare for the presentation.
   Read `PRESENTATION_QA.md` for like quesitons and anwer that may be asked

### For Reviewers:
- Read the two paired implementations carefully.
- Write or run one simple test to verify correctness.
- Check for input validation, memory safety, edge cases.
- Provide feedback to your pair.

### For Person 1 (Group Leader):
- Ensure all final implementations are integrated and tested.
- Coordinate team presentations and demo rehearsal.
- Present the architecture and integrated demo first (add → load → display → sort → save → exit).

---

## How We All Worked Together

The team followed a **modular, pair-based approach**: the group leader established the data model and main flow. Each pair independently implemented the same function; two shared reviewers compared both versions and selected the best based on quality and correctness. Everyone studied the leader's code to understand integration. All implementations were merged into a single, cohesive `student_record.c`. Each person prepared a presentation slide and demo to explain their function's design choices and edge cases, ensuring every team member contributed meaningfully to both code and presentation.

---

## Sample Leader Code Skeleton (Starting Point)

Below is a simplified version of the **group leader's starting code** that everyone used as a reference. It shows the core structures, basic helper functions (`displayMenu`, `displayWelcomeMessage`, `clearInputBuffer`, `confirmAction`), function declarations, and the `main` menu loop where each person plugs in their own function implementation.

```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX_NAME_LENGTH 50
#define PASSING_MARKS 40

// Data model
typedef struct {
    char name[50];
    int  rollNumber;
    float marks;
} Student;

typedef struct {
    Student *students;
    int count;
    int capacity;
} StudentRecordSystem;

// Function declarations (everyone implements one or more of these)
void initializeSystem(StudentRecordSystem *records);
void displayWelcomeMessage();
void displayMenu();
void addStudent(StudentRecordSystem *records);
void displayAllStudents(const StudentRecordSystem *records);
void searchStudent(const StudentRecordSystem *records);
void modifyStudent(StudentRecordSystem *records);
void removeStudent(StudentRecordSystem *records);
void calculateAverageMarks(const StudentRecordSystem *records);
void sortStudents(StudentRecordSystem *records, int ascending);
void saveToFile(const StudentRecordSystem *records);
void loadFromFile(StudentRecordSystem *records);
int  confirmAction(const char *actionDescription);
void clearInputBuffer();

int main(void) {
    StudentRecordSystem records;
    initializeSystem(&records);

    char userName[50];
    displayWelcomeMessage();
    printf("\nPlease enter your name: ");
    fgets(userName, sizeof(userName), stdin);
    userName[strcspn(userName, "\n")] = '\0';

    printf("\nHello, %s! Welcome to the Student Record System.\n\n", userName);

    int choice;
    do {
        displayMenu();
        printf("Enter your choice (1-11): ");

        if (scanf("%d", &choice) != 1) {
            printf("Invalid input! Please enter a number.\n");
            clearInputBuffer();
            continue;
        }
        clearInputBuffer();

        switch (choice) {
            case 1:
                if (!confirmAction("add a new student")) break;
                addStudent(&records);
                break;
            case 2:
                if (!confirmAction("display all students")) break;
                displayAllStudents(&records);
                break;
            case 3:
                if (!confirmAction("search for a student")) break;
                searchStudent(&records);
                break;
            case 4:
                if (!confirmAction("modify student records")) break;
                modifyStudent(&records);
                break;
            case 5:
                if (!confirmAction("remove a student")) break;
                removeStudent(&records);
                break;
            case 6:
                if (!confirmAction("calculate average marks")) break;
                calculateAverageMarks(&records);
                break;
            case 7:
                if (!confirmAction("sort students by marks in ascending order")) break;
                sortStudents(&records, 1);
                break;
            case 8:
                if (!confirmAction("sort students by marks in descending order")) break;
                sortStudents(&records, 0);
                break;
            case 9:
                if (!confirmAction("save records to a file")) break;
                saveToFile(&records);
                break;
            case 10:
                if (!confirmAction("load records from a file")) break;
                loadFromFile(&records);
                break;
            case 11:
                if (confirmAction("exit the program")) {
                    printf("Thank you for using the Student Record System, %s! Goodbye!\n", userName);
                } else {
                    choice = 0; // stay in menu
                }
                break;
            default:
                printf("Invalid choice! Please try again.\n");
        }
    } while (choice != 11);

    free(records.students);
    return 0;
}

void initializeSystem(StudentRecordSystem *records) {

    // Choose a small default capacity; realloc will grow if needed.
    records->capacity = 100;
    records->students = (Student*)malloc(records->capacity * sizeof(Student));
    records->count = 0;

    // Simple check: exit if heap allocation fails
    if (records->students == NULL) {
        printf("Memory allocation failed!\n");
        exit(1);
    }
}

void displayWelcomeMessage() {
    printf("========================================\n");
    printf("     STUDENT RECORD SYSTEM\n");
    printf("========================================\n");
}

void displayMenu() {
    printf("========================================\n");
    printf("            MAIN MENU\n");
    printf("========================================\n");
    printf("1. Add Student\n");
    printf("2. Display All Students\n");
    printf("3. Search Student by Roll Number\n");
    printf("4. Modify Student Record\n");
    printf("5. Remove Student\n");
    printf("6. Calculate Average Marks\n");
    printf("7. Sort Students by Marks (Ascending)\n");
    printf("8. Sort Students by Marks (Descending)\n");
    printf("9. Save Records to a Comma-Separated Text File (CSV-Style)\n");
    printf("10. Load Records from a Comma-Separated Text File (CSV-Style)\n");
    printf("11. Exit\n");
    printf("----------------------------------------\n");
}

int confirmAction(const char *actionDescription) {
    char buffer[16];
    printf("Are you sure you want to %s? Press y/Y to continue, n/N to cancel: ", actionDescription);
    if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
        printf("Action cancelled.\n");
        return 0;
    }
    if (buffer[0] == 'y' || buffer[0] == 'Y') {
        return 1;
    }
    printf("Action cancelled.\n");
    return 0;
}

void clearInputBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {
        /* discard extra characters */
    }
}
```

This skeleton shows clearly **where** each function is called and how user confirmation is handled, so every contributor can test their own function from the same entry point.

---

## Implementation Plan for Group Members

To keep everyone aligned, the group leader proposed this implementation plan:

1. **Understand the skeleton**  
   - Every member first reads the leader’s skeleton and the fully commented file `student_record_commented.c` to understand:
     - Data structures (`Student`, `StudentRecordSystem`).
     - The menu loop and how functions are called.
     - How confirmation (`confirmAction`) and input handling work.

2. **Implement your assigned function in isolation**  
   - Each pair (e.g., Persons 4 and 5 for `addStudent`) copies the function signature from the skeleton and implements it inside their own local file or branch.
   - They focus on:
     - Correct logic.
     - Input validation and error messages.
     - Handling edge cases listed in the documentation.

3. **Use the common `main` for testing**  
   - All contributors compile and run using the **same** `main` (the skeleton) so behavior is consistent.
   - Example: compile with `gcc -o student_record student_record.c` and test their menu option.

4. **Review and compare implementations**  
   - The assigned reviewers for each pair run both versions of the function.
   - They check:
     - Does it handle invalid input safely?
     - Does it match the behavior described in the documentation?
     - Is the code clear and well-structured?

5. **Select and integrate the best version**  
   - Reviewers and the group leader agree on which version to integrate.
   - The chosen implementation is merged into the main `student_record.c` in the appropriate function body.

6. **Final integration pass by the leader**  
   - The leader ensures:
     - All functions compile together.
     - There are no duplicate or unused functions.
     - Menu options, confirmation prompts, and file paths are consistent.

7. **Presentation preparation**  
   - Each member prepares a 1–2 minute explanation of **their** function:
     - What it does.
     - One design decision (e.g., why we used `fgets`, why we check range 0–100, why we used `qsort`).
     - One edge case it handles.
   - Everyone tests their menu option with the **confirmation prompts** turned on so they know how the program behaves when the user chooses "yes" or "no".
   - The leader presents the overall architecture and runs a full demo (add → display → search/modify/remove → sort → save/load → exit) including at least one cancelled action to show the confirmation dialog in action.
   Read `HOW_TO_PREPARE_FOR_PRESENTATION.md` for more details on how to prepare for the presentation.
   Read `PRESENTATION_QA.md` for like quesitons and anwer that may be asked


---

## Contacts

- **Person 1 (Leader):** Architecture, integration, rehearsal coordination.
- **Your 2 Shared Reviewers:** Code quality, testing, demo validation.
- **Everyone:** Prepare slides, study the code, participate in rehearsal.

**Goal:** Make it clear to the lecturer that every person contributed to design, implementation review, and presentation quality.