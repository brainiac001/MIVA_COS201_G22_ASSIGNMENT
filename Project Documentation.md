# Student Record System – Project Documentation

**Group:** COS201 G22  
**Course:** Programming Fundamentals (C)  
**Submission Date:** November 2025

---

## Table of Contents

1. [Group Details](#group-details)
2. [Requirements](#requirements)
3. [Planning and Considerations](#planning-and-considerations)
4. [Program Flow and User Story](#program-flow-and-user-story)
5. [Program Code](#program-code)
6. [Quick Code Explanation](#quick-code-explanation)
7. [Key Features and Edge Cases](#key-features-and-edge-cases)
8. [Conclusion](#conclusion)

---

## Group Details

**Group Number:** COS201 G22

**Team Members:**
- Person 1 (Group Leader)
- Persons 2, 3
- Persons 4, 5
- Persons 6, 7
- Persons 8, 9
- Persons 10, 11
- Persons 12, 13
- Persons 14, 15

**Project:** Student Record System (C)  
**Language:** C (ANSI C / C11)  
**Compiler:** GCC  
**Lines of Code:** ~400

---

## Requirements

### Project Title
**Student Record System**

### Project Brief
Create a simple Student Record System in C that allows users to manage student information. The program should include the following functionality:

### Initial Setup
- ✓ Create a new C project using JetBrains CLion (or preferred development environment)
- ✓ Write a program that displays a welcome message and prompts the user to enter their name
- ✓ The program should greet the user by their name

### Data Storage
- ✓ Implement storage for student information, including name, roll number, and marks, using appropriate data types

### Input and Output
- ✓ Add a feature to input a student's marks and display whether the student has passed or failed based on a passing threshold (marks above 40 are considered passing)

### Student Records Management
- ✓ Implement a loop that allows the user to input information for multiple students until the user decides to exit
- ✓ Create an array to store information for multiple students, with the ability to add, remove, and modify student records
- ✓ Implement functions for adding, modifying, and displaying student records
- ✓ Ensure functions take appropriate arguments and return relevant information

### Memory Management
- ✓ Use dynamic memory allocation for student records
- ✓ Ensure memory is freed when no longer needed

### File Operations
- ✓ Implement the ability to save student records to a text file
- ✓ Implement the ability to load student records from a text file
- ✓ Store information such as name, roll number, and marks in files

### Search Functionality
- ✓ Allow users to search for a student by their roll number
- ✓ Display the relevant information when a student is found

### Calculations and Sorting
- ✓ Calculate and display the average marks for all students
- ✓ Provide an option to sort student records based on marks in ascending order
- ✓ Provide an option to sort student records based on marks in descending order

### Course and Assessment Information
- **Course Code:** COS 201 – Computer Programming I
- **Assessment Type:** Tutor-Marked Assessment
- **Group:** COS201 G22

---

## Planning and Considerations

### System Architecture

We designed this system using a **modular, function-based approach**. Each operation is a separate function, making the code maintainable and testable. The program follows a **client-server pattern** where the `main` function acts as an orchestrator that reads user input and delegates work to specialized functions.

```
┌─────────────────────────────────────────┐
│         main() - CLI Orchestrator       │
│  (menu loop, user input, exit handling) │
└──────────────┬──────────────────────────┘
               │
      ┌────────┴────────┬─────────────┬──────────────┬──────────────┐
      │                 │             │              │              │
      ▼                 ▼             ▼              ▼              ▼
  ┌────────┐      ┌──────────┐  ┌─────────┐  ┌──────────────┐  ┌───────────┐
  │ CRUD   │      │I/O Ops   │  │Sorting  │  │Utilities     │  │Display    │
  ├────────┤      ├──────────┤  ├─────────┤  ├──────────────┤  ├───────────┤
  │Add     │      │Save      │  │Sort     │  │Initialize   │  │Show All   │
  │Search  │      │Load      │  │Compare  │  │Clear Buffer  │  │Menu       │
  │Modify  │      │          │  │         │  │             │  │Welcome    │
  │Remove  │      │          │  │         │  │             │  │           │
  └────────┘      └──────────┘  └─────────┘  └──────────────┘  └───────────┘
```

### Data Model

The system uses two main C structs:

**Student Record:**
```c
typedef struct {
    char name[50];        // Student's full name (max 50 chars)
    int rollNumber;       // Unique identifier
    float marks;          // Score out of 100
} Student;
```

**System State:**
```c
typedef struct {
    Student *students;    // Dynamic array of students
    int count;            // Number of students currently stored
    int capacity;         // Maximum students before resize
} StudentRecordSystem;
```

**Key Design Decisions:**
- **Dynamic Array:** Starts with capacity 100; doubles when full (prevents fixed-size limitations)
- **Pass/Fail Status:** Computed on-the-fly (not stored) as `marks >= 40 ? "PASS" : "FAIL"`
- **CSV Format:** For file storage (easy to view in Excel, portable across systems)
- **Input Validation:** Each function validates data before storage

### Code Review Strategy

The team followed a **pair-based review model**:
- Two developers implement the same function independently
- Two shared reviewers compare both implementations and select the best
- All team members studied the group leader's `main` function to ensure architectural alignment
- Quality criteria: correctness, error handling, code clarity, efficiency

---

## Program Flow and User Story

### System Initialization and Main Loop

When the program starts, it follows this sequence:

```
START
  │
  ├─► Initialize system (allocate array, set capacity=100, count=0)
  │
  ├─► Display welcome banner
  │
  ├─► Ask user for their name
  │
  ├─► Enter MENU LOOP
  │    │
  │    ├─► Display menu (11 options)
  │    │
  │    ├─► Read user choice (1-11)
  │    │
  │    ├─► Execute chosen operation
  │    │
  │    ├─► Ask "Continue? (y/n)"
  │    │
  │    └─► If 'n' or 'N' → exit loop, else repeat
  │
  ├─► Free all allocated memory
  │
  └─► EXIT
```

### User Story: Adding and Sorting Students

**Scenario:** A teacher wants to manage a class of 50 students.

1. **Start Program** → Greets teacher, shows menu
2. **Add Students** → Teacher selects option 1, enters 50 students one by one
   - System validates each name (non-empty), roll number (positive, no duplicates), and marks (0–100)
   - Array auto-resizes from 100→100→100... (no resize needed in this case)
3. **Display All** → Teacher selects option 2, sees formatted table with pass/fail status
4. **Sort Descending** → Teacher selects option 8, students sorted by marks (highest first)
5. **Save to File** → Teacher selects option 9, saves to `class_records.csv`
6. **Exit** → Teacher selects option 11, program frees memory and closes

### Data Flow Diagram: Adding a Student

```
User Input (name, roll, marks)
            │
            ▼
┌─────────────────────────────┐
│ Validation Layer            │
│ • Name: non-empty?          │
│ • Roll: positive integer?   │
│ • Marks: 0-100?            │
│ • Duplicate roll number?    │
└──────────┬──────────────────┘
           │ (Valid)
           ▼
┌─────────────────────────────┐
│ Capacity Check              │
│ • count >= capacity?        │
│ • If YES: resize array ×2   │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│ Store in Array              │
│ students[count++] = newStud │
└──────────┬──────────────────┘
           │
           ▼
       Confirm "Added!"
```

---

## Program Code

The complete implementation is provided in `student_record.c`. The code is organized into three main sections:

### 1. Data Structures and Constants
- `MAX_NAME_LENGTH = 50`
- `PASSING_MARKS = 40`
- `Student` struct
- `StudentRecordSystem` struct

### 2. Function Declarations (14 functions total)
Listed in order of invocation and complexity:
1. `initializeSystem()` – Set up empty system
2. `displayWelcomeMessage()` – Show banner
3. `displayMenu()` – Show 11-option menu
4. `main()` – CLI orchestration
5. `addStudent()` – CRUD: Create
6. `displayAllStudents()` – Read
7. `searchStudent()` – Read (single)
8. `modifyStudent()` – Update
9. `removeStudent()` – Delete
10. `calculateAverageMarks()` – Analytics
11. `sortStudents()` – Sort with qsort
12. `compareMarks()` – Helper for qsort
13. `saveToFile()` – Persist to CSV
14. `loadFromFile()` – Load from CSV
15. `clearInputBuffer()` – Input utility

### 3. Main Function
The `main()` function implements the CLI loop:
```c
int main() {
    StudentRecordSystem records;
    initializeSystem(&records);
    
    // Prompt for user name
    displayWelcomeMessage();
    printf("Please enter your name: ");
    fgets(userName, sizeof(userName), stdin);
    
    // Loop: show menu → read choice → execute action → ask continue
    do {
        displayMenu();
        printf("Enter your choice (1-11): ");
        scanf("%d", &choice);
        
        // Switch on choice: case 1 (add), case 2 (display), ..., case 11 (exit)
        switch (choice) { ... }
        
        // Continue prompt
        if (choice != 11) {
            printf("Do you want to continue? (y/n): ");
            if (contBuf[0] == 'n' || contBuf[0] == 'N') choice = 11;
        }
    } while (choice != 11);
    
    free(records.students);
    return 0;
}
```

---

## Quick Code Explanation

### 1. `initializeSystem()` – Memory Setup
**What it does:** Allocates memory for the student array and initializes the system state.

```c
void initializeSystem(StudentRecordSystem *records) {
    records->capacity = 100;
    records->students = (Student*)malloc(records->capacity * sizeof(Student));
    records->count = 0;
    if (records->students == NULL) {
        printf("Memory allocation failed!\n");
        exit(1);
    }
}
```

**Design Choices:**
- Starts with capacity 100 (reasonable default, avoids immediate resizing)
- Exits immediately if malloc fails (rather than continue with bad state)
- Count set to 0 (no students initially)

**Edge Case:** Handles malloc failure gracefully by printing an error and exiting, preventing null-pointer dereferences later.

---

### 2. `addStudent()` – Input Validation & Storage
**What it does:** Prompts for student details, validates them, checks for duplicates, resizes if needed, and stores the record.

```c
void addStudent(StudentRecordSystem *records) {
    if (records->count >= records->capacity) {
        records->capacity *= 2;
        records->students = (Student*)realloc(records->students, 
                                               records->capacity * sizeof(Student));
    }
    
    // Input: name, roll number, marks
    // Validation: roll number > 0, no duplicates, marks 0-100
    
    records->students[records->count] = newStudent;
    records->count++;
}
```

**Design Choices:**
- Validates roll number before checking duplicates (fail-fast)
- Uses `fgets()` for name input (safer than `scanf()` for strings)
- Marks validated as 0–100 (realistic range)
- Auto-resizes array when full

**Edge Cases:**
- Duplicate roll number: Rejects and asks to re-enter
- Invalid marks (negative or > 100): Prompts again
- Array capacity exhausted: Automatically resizes (×2)

---

### 3. `displayAllStudents()` – Formatted Output
**What it does:** Displays all students in a table with columns for serial number, name, roll number, marks, and pass/fail status.

```c
void displayAllStudents(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    printf("%-5s %-20s %-12s %-8s %s\n", "S.No", "Name", "Roll Number", "Marks", "Status");
    
    for (int i = 0; i < records->count; i++) {
        const Student *student = &records->students[i];
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
        
        printf("%-5d %-20s %-12d %-8.2f %s\n", i + 1, student->name, 
               student->rollNumber, student->marks, status);
    }
}
```

**Design Choices:**
- Uses format specifiers like `%-20s` (left-aligned, 20-char width) for clean table formatting
- Pass/fail status computed on-the-fly (not stored, saves memory)
- 1-indexed serial number for user readability (humans think "student 1," not "student 0")

**Edge Case:** Handles empty record set gracefully with a friendly message instead of crashing or printing garbage.

---

### 4. `searchStudent()` – Lookup by Roll Number
**What it does:** Searches for a student by roll number and displays details if found.

```c
void searchStudent(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    int rollNumber;
    printf("Enter roll number to search: ");
    scanf("%d", &rollNumber);
    
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == rollNumber) {
            // Display found student
            return;
        }
    }
    
    printf("Student with roll number %d not found.\n", rollNumber);
}
```

**Design Choices:**
- Linear search (sufficient for typical class sizes; O(n) complexity)
- Checks for empty record first (prevents unnecessary loop)
- Friendly "not found" message

**Edge Case:** Returns after finding first match (roll numbers are unique, so at most one match exists).

---

### 5. `modifyStudent()` – Partial Updates
**What it does:** Allows updating a student's name or marks without changing other fields.

```c
void modifyStudent(StudentRecordSystem *records) {
    // Find student by roll number
    
    printf("Enter new name (or press Enter to keep current): ");
    fgets(newName, sizeof(newName), stdin);
    if (strlen(newName) > 0) {
        strcpy(records->students[i].name, newName);
    }
    
    printf("Enter new marks (or press Enter to keep current): ");
    // Parse as float, validate 0-100, update if valid
}
```

**Design Choices:**
- Allows **empty input to skip a field** (user can press Enter without changing)
- Uses `strtof()` for safe float parsing
- Displays current values before prompting (user knows what they're changing)

**Edge Case:** User presses Enter for both fields → no change made (idempotent).

---

### 6. `removeStudent()` – Array Element Deletion
**What it does:** Removes a student by roll number, shifting remaining elements left.

```c
void removeStudent(StudentRecordSystem *records) {
    // Find student by roll number
    
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == rollNumber) {
            // Shift elements after i one position left
            for (int j = i; j < records->count - 1; j++) {
                records->students[j] = records->students[j + 1];
            }
            records->count--;
            printf("Student removed successfully!\n");
            return;
        }
    }
}
```

**Design Choices:**
- In-place deletion (no temporary array needed, O(n) but space-efficient)
- Decrements count to mark the array slot as unused
- Preserves order of remaining students

**Edge Case:** Removal of the last student leaves no gap; shift loop simply doesn't execute.

---

### 7. `calculateAverageMarks()` – Simple Statistics
**What it does:** Computes and displays the average marks of all students.

```c
void calculateAverageMarks(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    float total = 0;
    for (int i = 0; i < records->count; i++) {
        total += records->students[i].marks;
    }
    
    float average = total / records->count;
    printf("Average marks: %.2f\n", average);
}
```

**Design Choices:**
- Formats output as `%.2f` (2 decimal places, e.g., 75.50)
- Total starts at 0 (avoids garbage values)
- Empty check prevents division by zero

**Edge Case:** With one student, average equals that student's marks (correct behavior).

---

### 8. `sortStudents()` – qsort Integration
**What it does:** Sorts students by marks in ascending or descending order using the standard C `qsort()` function.

```c
static int g_sortAscending = 1;  // Global flag for comparator

static int compareMarks(const void *a, const void *b) {
    const Student *sa = (const Student *)a;
    const Student *sb = (const Student *)b;
    
    if (sa->marks < sb->marks) return g_sortAscending ? -1 : 1;
    if (sa->marks > sb->marks) return g_sortAscending ? 1 : -1;
    return 0;  // Equal marks
}

void sortStudents(StudentRecordSystem *records, int ascending) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    g_sortAscending = ascending ? 1 : 0;
    qsort(records->students, records->count, sizeof(Student), compareMarks);
    
    printf("Students sorted by marks in %s order.\n", ascending ? "ascending" : "descending");
    displayAllStudents(records);
}
```

**Design Choices:**
- Uses `qsort()` from stdlib (proven, O(n log n) average case)
- Global `g_sortAscending` flag allows comparator to handle both directions without duplicating code
- Calls `displayAllStudents()` immediately after sort (confirm results to user)

**Edge Case:** Sorting a single student succeeds (qsort handles edge case, no comparison needed).

---

### 9. `saveToFile()` – CSV Export
**What it does:** Exports all student records to a CSV file with headers and computed pass/fail status.

```c
void saveToFile(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students to save.\n");
        return;
    }
    
    char filename[100];
    printf("Enter filename to save records: ");
    fgets(filename, sizeof(filename), stdin);
    
    FILE *file = fopen(filename, "w");
    if (file == NULL) {
        printf("Error opening file for writing!\n");
        return;
    }
    
    fprintf(file, "Name,RollNumber,Marks,Status\n");  // Header
    
    for (int i = 0; i < records->count; i++) {
        const Student *student = &records->students[i];
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
        
        fprintf(file, "%s,%d,%.2f,%s\n", student->name, student->rollNumber, 
                student->marks, status);
    }
    
    fclose(file);
    printf("Student records saved to '%s' successfully!\n", filename);
}
```

**Design Choices:**
- CSV format (comma-separated values) for portability to Excel/Google Sheets
- Header row for clarity
- Pass/fail status included (convenience for users)
- Error handling: checks if file open fails

**Edge Case:** Filename with path (e.g., `./records/students.csv`) works if directory exists.

---

### 10. `loadFromFile()` – CSV Import
**What it does:** Imports student records from a CSV file, parsing each line and populating the array.

```c
void loadFromFile(StudentRecordSystem *records) {
    char filename[100];
    printf("Enter filename to load records: ");
    fgets(filename, sizeof(filename), stdin);
    
    FILE *file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error opening file for reading!\n");
        return;
    }
    
    records->count = 0;  // Clear existing records
    
    char line[256];
    int lineNumber = 0;
    
    while (fgets(line, sizeof(line), file)) {
        lineNumber++;
        
        if (lineNumber == 1) continue;  // Skip header
        
        line[strcspn(line, "\n")] = '\0';  // Remove newline
        
        // Parse CSV: Name,RollNumber,Marks,Status
        char *token = strtok(line, ",");
        if (token == NULL) continue;
        
        if (records->count >= records->capacity) {
            records->capacity *= 2;
            records->students = (Student*)realloc(records->students, 
                                                   records->capacity * sizeof(Student));
        }
        
        strcpy(records->students[records->count].name, token);
        
        token = strtok(NULL, ",");
        records->students[records->count].rollNumber = atoi(token);
        
        token = strtok(NULL, ",");
        records->students[records->count].marks = atof(token);
        
        records->count++;
    }
    
    fclose(file);
    printf("Student records loaded from '%s' successfully! (%d students loaded)\n", 
           filename, records->count);
    
    if (records->count > 0) {
        displayAllStudents(records);
    }
}
```

**Design Choices:**
- Uses `strtok()` for line-by-line CSV parsing
- Skips header row (lineNumber == 1)
- Resizes array if needed during load
- Displays loaded records immediately (confirmation)
- Clears existing records before loading (prevents mixing old and new data)

**Edge Cases:**
- Missing or malformed CSV: Skipped with `if (token == NULL) continue`
- Large file: Auto-resizing handles any number of records
- Status column ignored during parsing (not stored, computed on-the-fly)

---

### 11. `clearInputBuffer()` – Input Cleanup
**What it does:** Clears remaining characters from the input buffer (e.g., after `scanf()` leaves a newline).

```c
void clearInputBuffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
```

**Design Choices:**
- Reads characters until newline (consumed) or EOF
- Prevents "leftover" input from affecting next prompt

**Edge Case:** End-of-file (EOF): Loop exits cleanly without infinite loop.

---

### 12. Helper Functions – UI Elements
- **`displayWelcomeMessage()`** – Prints a banner with "STUDENT RECORD SYSTEM" title
- **`displayMenu()`** – Lists all 11 menu options in a formatted box

These functions improve UX by presenting a professional, organized interface.

---

## Key Features and Edge Cases

### Features Implemented

| Feature | Function | Status |
|---------|----------|--------|
| Add student with validation | `addStudent()` | ✓ Implemented |
| Display formatted table | `displayAllStudents()` | ✓ Implemented |
| Search by roll number | `searchStudent()` | ✓ Implemented |
| Modify name or marks | `modifyStudent()` | ✓ Implemented |
| Remove student | `removeStudent()` | ✓ Implemented |
| Calculate average marks | `calculateAverageMarks()` | ✓ Implemented |
| Sort ascending/descending | `sortStudents()` | ✓ Implemented |
| Save to CSV | `saveToFile()` | ✓ Implemented |
| Load from CSV | `loadFromFile()` | ✓ Implemented |
| Dynamic array resizing | `realloc()` | ✓ Implemented |
| Pass/fail status | `displayAllStudents()` | ✓ Implemented |
| CLI menu loop | `main()` | ✓ Implemented |

### Edge Cases Handled

1. **Empty Record Set** – All display/analyze functions check `if (count == 0)` and print friendly message
2. **Duplicate Roll Numbers** – `addStudent()` prevents duplicate roll numbers
3. **Array Capacity Exceeded** – `realloc()` automatically doubles capacity
4. **Invalid Input** – Marks validated 0–100, roll numbers > 0, names non-empty
5. **File I/O Errors** – `fopen()` checked; error message printed if file not found
6. **Malformed CSV** – `strtok()` validated; missing fields skipped
7. **Memory Allocation Failure** – `malloc()` checked; program exits with error message
8. **Partial Modification** – User can press Enter to skip a field (partial updates supported)
9. **Character Buffer Overflow** – `fgets()` used instead of `gets()` (fixed-size buffers)
10. **Input Buffer Pollution** – `clearInputBuffer()` called after `scanf()` to remove trailing newline

---

## Conclusion

The **Student Record System** is a robust, modular C application that demonstrates core programming concepts: dynamic memory management, input validation, file I/O, sorting algorithms, and user interface design. 

### What We Learned

1. **Modular Design:** Separating concerns (CRUD, I/O, sorting, UI) makes code maintainable and testable.
2. **Error Handling:** Validating input and checking return values (malloc, fopen) prevents crashes.
3. **User Experience:** Clear prompts, formatted output, and graceful handling of edge cases build trust.
4. **Memory Safety:** Using `fgets()` over `gets()`, checking buffer sizes, and freeing memory prevents vulnerabilities.
5. **Standard Library Functions:** `qsort()`, `strtok()`, `realloc()` are powerful tools when used correctly.

### Testing Recommendations

To verify correctness:
1. **Add 5 students** with various marks (30, 50, 75, 90, 10) → Check pass/fail status
2. **Search for non-existent roll number** → Verify "not found" message
3. **Sort ascending, then descending** → Verify order changes
4. **Save to file, then load** → Verify all records preserved
5. **Add 105 students** → Verify array auto-resizes beyond initial 100 capacity
6. **Modify a student and leave marks blank** → Verify marks unchanged
7. **Remove middle student** → Verify others shift correctly
8. **Calculate average** → Verify math is correct

### Future Enhancements

- **Sorting by name or roll number** (not just marks)
- **Delete by name** (not just roll number)
- **Grade statistics** (min, max, median, standard deviation)
- **Batch operations** (import 1000+ records from CSV)
- **Search by name** (partial match, case-insensitive)
- **User authentication** (admin vs. viewer roles)
- **Persistent backup** (version control for records)

---

**End of Documentation**

*This project was developed by COS201 Group 22 as a foundational exercise in systems programming, data structures, and software design.*
