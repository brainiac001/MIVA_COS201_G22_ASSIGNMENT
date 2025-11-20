# Student Record System – Project Documentation

**Group:**  Group 22  
**Course:** Programming Fundamentals (C)  

---

## Table of Contents

1. [Group Details](#1-group-details)
1. [Requirements](#2-requirements)
1. [Planning and Considerations](#3-planning-and-considerations)
1. [Program Flow and User Story](#4-program-flow-and-user-story)
1. [Program Code](#5-program-code)
1. [Quick Code Explanation](#6-quick-code-explanation)
1. [Key Features–Function Map and Edge Cases](#7-key-features-function-map-and-edge-cases)
1. [Conclusion](#8-conclusion)

---

## 1. Group Details

**Group Number:** Group 22 

**Team Members:**

| Student Name                   | Student ID   | School Email                   |
|--------------------------------|--------------|--------------------------------|
| Person 1 (Group Leader)        |              |                                |
| Person 2                       |              |                                |
| Person 3                       |              |                                |
| Person 4                       |              |                                |
| Person 5                       |              |                                |
| Person 6                       |              |                                |
| Person 7                       |              |                                |
| Person 8                       |              |                                |
| Person 9                       |              |                                |
| Person 10                      |              |                                |
| Person 11                      |              |                                |
| Person 12                      |              |                                |
| Person 13                      |              |                                |
| Person 14                      |              |                                |
| Person 15                      |              |                                |


**Project:** Student Record System (C)  
**Language:** C (ANSI C / C11)  

---

## 2. Requirements

### Project Title
**Student Record System**

### Project Brief
Create a simple Student Record System in C that allows users to manage student information. The program should include the following functionality:

### Initial Setup
- Create a new C project using JetBrains CLion (or preferred development environment)
- Write a program that displays a welcome message and prompts the user to enter their name
- The program should greet the user by their name

### Data Storage
- Implement storage for student information, including name, roll number, and marks, using appropriate data types

### Input and Output
- Add a feature to input a student's marks and display whether the student has passed or failed based on a passing threshold (marks above 40 are considered passing)

### Student Records Management
- Implement a loop that allows the user to input information for multiple students until the user decides to exit
- Create an array to store information for multiple students, with the ability to add, remove, and modify student records
- Implement functions for adding, modifying, and displaying student records
- Ensure functions take appropriate arguments and return relevant information

### Memory Management
- Use dynamic memory allocation for student records
- Ensure memory is freed when no longer needed

### File Operations
- Implement the ability to save student records to a text file
- Implement the ability to load student records from a text file
- Store information such as name, roll number, and marks in files

### Search Functionality
- Allow users to search for a student by their roll number
- Display the relevant information when a student is found

### Calculations and Sorting
- Calculate and display the average marks for all students
- Provide an option to sort student records based on marks in ascending order
- Provide an option to sort student records based on marks in descending order

### Course and Assessment Information
- **Course Code:** COS 201 – Computer Programming I
- **Assessment Type:** Tutor-Marked Assessment
- **Group:** COS201 G22

---

## 3. Planning and Considerations

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
  ┌────────┐      ┌──────────┐  ┌─────────┐  ┌─────────────┐  ┌───────────┐
  │ CRUD   │      │I/O Ops   │  │Sorting  │  │Utilities    │  │Display    │
  ├────────┤      ├──────────┤  ├─────────┤  ├─────────────┤  ├───────────┤
  │Add     │      │Save      │  │Sort     │  │Initialize   │  │Show All   │
  │Search  │      │Load      │  │Compare  │  │Clear Buffer │  │Menu       │
  │Modify  │      │          │  │         │  │             │  │Welcome    │
  │Remove  │      │          │  │         │  │             │  │           │
  └────────┘      └──────────┘  └─────────┘  └─────────────┘  └───────────┘
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

## 4. Program Flow and User Story

### Detailed Program Flow and Menu Loop

When the program starts, it follows this overall sequence:

```
START
  │
  ├─► Initialize system (allocate array, set capacity=100, count=0)
  │
  ├─► Display welcome banner
  │
  ├─► Ask user for their name
  │
  ├─► Enter MAIN MENU LOOP
  │    │
  │    ├─► Display menu (options 1–11)
  │    │
  │    ├─► Read user choice (1–11)
  │    │
  │    ├─► Validate input
  │    │    │
  │    │    ├─► If invalid (non‑number or out of range)
  │    │    │     └─► Show error → return to menu
  │    │    │
  │    │    └─► If valid (1–11)
  │    │
  │    ├─► Execute chosen operation
  │    │    │
  │    │    ├─► 1: Add student
  │    │    │        • Prompt for name, roll number, and marks
  │    │    │        • Validate roll number (> 0 and not already used)
  │    │    │        • Validate marks (0–100)
  │    │    │        • Grow array with realloc() if capacity is full
  │    │    │        • Store new Student in records and increase count
  │    │    │
  │    │    ├─► 2: Display all students
  │    │    │        • Check if there is at least one student
  │    │    │        • Print table header (S/No, Name, Roll, Marks, Status)
  │    │    │        • For each student, compute PASS/FAIL and print row
  │    │    │
  │    │    ├─► 3: Search student by roll number
  │    │    │        • Ask for roll number
  │    │    │        • Linearly scan list for matching roll
  │    │    │        • If found, print full details and PASS/FAIL status
  │    │    │        • If not found, show "Student not found" message
  │    │    │
  │    │    ├─► 4: Modify existing student record
  │    │    │        • Ask for roll number and locate the student
  │    │    │        • Show current name and marks
  │    │    │        • Read new name (or Enter to keep existing)
  │    │    │        • Read new marks text; parse and validate if provided
  │    │    │        • Update only the fields the user changed
  │    │    │
  │    │    ├─► 5: Remove student from list
  │    │    │        • Ask for roll number
  │    │    │        • Find matching index in the array
  │    │    │        • Shift later elements one position left
  │    │    │        • Decrease count so the last slot is ignored
  │    │    │
  │    │    ├─► 6: Calculate average marks
  │    │    │        • Check that at least one student exists
  │    │    │        • Sum marks for all students
  │    │    │        • Divide by count and print the average
  │    │    │
  │    │    ├─► 7: Sort students by marks (ascending)
  │    │    │        • Set global flag g_sortAscending = 1
  │    │    │        • Call qsort() with compareMarks() helper
  │    │    │        • Show the sorted table using displayAllStudents()
  │    │    │
  │    │    ├─► 8: Sort students by marks (descending)
  │    │    │        • Set global flag g_sortAscending = 0
  │    │    │        • Call qsort() with compareMarks() helper
  │    │    │        • Show the sorted table using displayAllStudents()
  │    │    │
  │    │    ├─► 9: Save records to CSV file
  │    │    │        • Check if there is at least one student to save
  │    │    │        • Ask for filename from user
  │    │    │        • Open file in write mode and print CSV header
  │    │    │        • For each student, compute PASS/FAIL and write line
  │    │    │        • Close file and confirm save success
  │    │    │
  │    │    ├─► 10: Load records from CSV file
  │    │    │        • Ask for filename from user
  │    │    │        • Open file in read mode
  │    │    │        • Reset count to 0 before loading
  │    │    │        • Read file line by line, skipping the header row
  │    │    │        • Parse each line into name, roll, and marks
  │    │    │        • Grow array with realloc() if capacity is full
  │    │    │        • Store each parsed student and increase count
  │    │    │        • After loading, display all students for confirmation
  │    │    │
  │    │    └─► 11: Exit program
  │    │
  │    ├─► If choice == 11
  │    │      └─► Skip continue prompt and leave loop
  │    │
  │    ├─► Otherwise (choice 1–10)
  │    │    │
  │    │    ├─► Ask "Do you want to continue? (y/n)"
  │    │    │
  │    │    ├─► If answer is 'n' or 'N'
  │    │    │      └─► Set choice = 11 → exit loop
  │    │    │
  │    │    └─► Else (answer 'y' or other) → repeat menu
  │
  ├─► Free all allocated memory for student records
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
│ • Marks: 0-100?             │
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

## 5. Program Code
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>


//  Student Record System program to store students (name, roll number, marks)


#define MAX_NAME_LENGTH 50
#define PASSING_MARKS 40


// student data structure
typedef struct {
    char name[MAX_NAME_LENGTH];
    int rollNumber;
    float marks;
} Student;


//Holds the entire collection of students and metadata about the array
typedef struct {
    Student *students;
    int count;
    int capacity;
} StudentRecordSystem;


// Function declarations
void initializeSystem(StudentRecordSystem *records);
void displayWelcomeMessage();
void addStudent(StudentRecordSystem *records);
void displayAllStudents(const StudentRecordSystem *records);
void searchStudent(const StudentRecordSystem *records);
void modifyStudent(StudentRecordSystem *records);
void removeStudent(StudentRecordSystem *records);
void calculateAverageMarks(const StudentRecordSystem *records);
void sortStudents(StudentRecordSystem *records, int ascending);
void saveToFile(const StudentRecordSystem *records);
void loadFromFile(StudentRecordSystem *records);
void displayMenu();
void clearInputBuffer();


//main point of entry and runs the CLI loop
int main() {
  
    StudentRecordSystem records;
    initializeSystem(&records);
    
    char userName[50];
    displayWelcomeMessage();
    printf("\nPlease enter your name: ");


    fgets(userName, sizeof(userName), stdin);
    userName[strcspn(userName, "\n")] = '\0'; // trim username and remove newline
    
    printf("\nHello, %s! Welcome to the Student Record System.\n\n", userName);
    
    //loop to display the menu and get user choice
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
                addStudent(&records);
                break;
            case 2:
                displayAllStudents(&records);
                break;
            case 3:
                searchStudent(&records);
                break;
            case 4:
                modifyStudent(&records);
                break;
            case 5:
                removeStudent(&records);
                break;
            case 6:
                calculateAverageMarks(&records);
                break;
            case 7:
                sortStudents(&records, 1); // Ascending
                break;
            case 8:
                sortStudents(&records, 0); // Descending
                break;
            case 9:
                saveToFile(&records);
                break;
            case 10:
                loadFromFile(&records);
                break;
            case 11:
                printf("Thank you for using the Student Record System, %s! Goodbye!\n", userName);
                break;
            default:
                printf("Invalid choice! Please try again.\n");
        }
        printf("\n");


        // After performing the chosen action, ask the user if they want to continue or exit. if the user presses n or N, exit the program.
        if (choice != 11) {
            char contBuf[8];
            printf("Do you want to continue? (y/n): ");
            if (fgets(contBuf, sizeof(contBuf), stdin) != NULL) {
                if (contBuf[0] == 'n' || contBuf[0] == 'N') {
                    choice = 11; // set to exit
                    printf("Exiting as requested.\n");
                }
            }
        }
    } while (choice != 11);
    
    // Free dynamically allocated memory we created in initializeSystem.
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
    printf("9. Save Records to File\n");
    printf("10. Load Records from File\n");
    printf("11. Exit\n");
    printf("----------------------------------------\n");
}


//function to add new student, their name roll number and marks, it also checks if the student already exists
void addStudent(StudentRecordSystem *records) {
    if (records->count >= records->capacity) {
        // Resize the array
        records->capacity *= 2;
        records->students = (Student*)realloc(records->students, records->capacity * sizeof(Student));
        if (records->students == NULL) {
            printf("Memory allocation failed!\n");
            return;
        }
    }
    
    Student newStudent;
    
    printf("\n--- Add New Student ---\n");
    
    printf("Enter student name: ");
    fgets(newStudent.name, sizeof(newStudent.name), stdin);
    newStudent.name[strcspn(newStudent.name, "\n")] = '\0';
    
    printf("Enter roll number: ");
    while (scanf("%d", &newStudent.rollNumber) != 1 || newStudent.rollNumber <= 0) {
        printf("Invalid roll number! Please enter a positive integer: ");
        clearInputBuffer();
    }
    clearInputBuffer();
    
    // Check if roll number already exists
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == newStudent.rollNumber) {
            printf("Error: Roll number %d already exists!\n", newStudent.rollNumber);
            return;
        }
    }
    
    printf("Enter marks: ");
    while (scanf("%f", &newStudent.marks) != 1 || newStudent.marks < 0 || newStudent.marks > 100) {
        printf("Invalid marks! Please enter a value between 0 and 100: ");
        clearInputBuffer();
    }
    clearInputBuffer();
    
    records->students[records->count] = newStudent;
    records->count++;
    
    printf("Student added successfully!\n");
}


//function to display all students like a table, with appropriate spacing by using things like %-5s, %-12d etc.
void displayAllStudents(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    printf("\n--- All Students ---\n");
    printf("%-5s %-20s %-12s %-8s %s\n", "S.No", "Name", "Roll Number", "Marks", "Status");
    printf("------------------------------------------------------------\n");
    
    for (int i = 0; i < records->count; i++) {
        const Student *student = &records->students[i];
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
        
        printf("%-5d %-20s %-12d %-8.2f %s\n", 
               i + 1, student->name, student->rollNumber, student->marks, status);
    }
}


//function to search student by roll number and it checks if record is empty before searching
void searchStudent(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    int rollNumber;
    printf("\n--- Search Student ---\n");
    printf("Enter roll number to search: ");
    
    if (scanf("%d", &rollNumber) != 1) {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();
    
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == rollNumber) {
            const Student *student = &records->students[i];
            char status[10];
            strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
            
            printf("\nStudent Found:\n");
            printf("Name: %s\n", student->name);
            printf("Roll Number: %d\n", student->rollNumber);
            printf("Marks: %.2f\n", student->marks);
            printf("Status: %s\n", status);
            return;
        }
    }
    
    printf("Student with roll number %d not found.\n", rollNumber);
}


//function to modify student by using their roll number
void modifyStudent(StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    int rollNumber;
    printf("\n--- Modify Student Record ---\n");
    printf("Enter roll number of student to modify: ");
    
    if (scanf("%d", &rollNumber) != 1) {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();
    
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == rollNumber) {
            printf("Student found. Current details:\n");
            printf("Name: %s\n", records->students[i].name);
            printf("Roll Number: %d\n", records->students[i].rollNumber);
            printf("Marks: %.2f\n", records->students[i].marks);
            
            printf("\nEnter new name (or press Enter to keep current): ");
            char newName[MAX_NAME_LENGTH];
            fgets(newName, sizeof(newName), stdin);
            newName[strcspn(newName, "\n")] = '\0';
            
            if (strlen(newName) > 0) {
                strcpy(records->students[i].name, newName);
            }
            
            printf("Enter new marks (or press Enter to keep current): ");
            char marksInput[32];
            if (fgets(marksInput, sizeof(marksInput), stdin) != NULL) {
                marksInput[strcspn(marksInput, "\n")] = '\0';
                if (strlen(marksInput) == 0) {
                    printf("No marks entered; keeping current marks.\n");
                } else {
                    char *endptr;
                    float newMarks = strtof(marksInput, &endptr);
                    if (endptr != marksInput && *endptr == '\0') {
                        if (newMarks >= 0 && newMarks <= 100) {
                            records->students[i].marks = newMarks;
                        } else {
                            printf("Invalid marks! Keeping current marks.\n");
                        }
                    } else {
                        printf("Invalid marks input! Keeping current marks.\n");
                    }
                }
            }
            
            printf("Student record updated successfully!\n");
            return;
        }
    }
    
    printf("Student with roll number %d not found.\n", rollNumber);
}


//function to remove student by roll number
void removeStudent(StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }
    
    int rollNumber;
    printf("\n--- Remove Student ---\n");
    printf("Enter roll number of student to remove: ");
    
    if (scanf("%d", &rollNumber) != 1) {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();
    
    for (int i = 0; i < records->count; i++) {
        if (records->students[i].rollNumber == rollNumber) {
            // Shift all elements after the found student one position left
            for (int j = i; j < records->count - 1; j++) {
                records->students[j] = records->students[j + 1];
            }
            records->count--;
            printf("Student with roll number %d removed successfully!\n", rollNumber);
            return;
        }
    }
    
    printf("Student with roll number %d not found.\n", rollNumber);
}


//function to calculate average score of all students
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
    printf("\n--- Average Marks ---\n");
    printf("Total students: %d\n", records->count);
    printf("Average marks: %.2f\n", average);
}


//global variable to indicate ascending or descending sort
static int g_sortAscending = 1;


//helper function for qsort to compare marks, we need this function to
//tell qsort what field to sort on and in which direction
static int compareMarks(const void *a, const void *b) {
    // The comparator  returns negative/zero/positive to indicate ordering to qsort.
    const Student *sa = (const Student *)a;
    const Student *sb = (const Student *)b;


    if (sa->marks < sb->marks) return g_sortAscending ? -1 : 1;
    if (sa->marks > sb->marks) return g_sortAscending ? 1 : -1;
    return 0; // equal marks
}


//function to sort students by marks in ascending or descending order using qsort
void sortStudents(StudentRecordSystem *records, int ascending) {
    if (records->count == 0) {
        printf("No students in the record system.\n");
        return;
    }


    // Set the global flag then call qsort with the single comparator.
    g_sortAscending = ascending ? 1 : 0;
    qsort(records->students, records->count, sizeof(Student), compareMarks);


    printf("Students sorted by marks in %s order.\n", ascending ? "ascending" : "descending");
    displayAllStudents(records);
}


//function to save to file with proper file handling and CSV format
void saveToFile(const StudentRecordSystem *records) {
    if (records->count == 0) {
        printf("No students to save.\n");
        return;
    }
    
    char filename[100];
    printf("\n--- Save to File ---\n");
    printf("Enter filename to save records: ");
    fgets(filename, sizeof(filename), stdin);
    filename[strcspn(filename, "\n")] = '\0';
    
    FILE *file = fopen(filename, "w");
    if (file == NULL) {
        printf("Error opening file for writing!\n");
        return;
    }
    
    // Write header
    fprintf(file, "Name,RollNumber,Marks,Status\n");
    
    // Write student data
    for (int i = 0; i < records->count; i++) {
        const Student *student = &records->students[i];
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
        
        fprintf(file, "%s,%d,%.2f,%s\n", 
                student->name, student->rollNumber, student->marks, status);
    }
    
    fclose(file);
    printf("Student records saved to '%s' successfully!\n", filename);
}



void loadFromFile(StudentRecordSystem *records) {
    char filename[100];
    printf("\n--- Load from File ---\n");
    printf("Enter filename to load records: ");
    fgets(filename, sizeof(filename), stdin);
    filename[strcspn(filename, "\n")] = '\0';
    
    FILE *file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error opening file for reading!\n");
        return;
    }
    
    // Clear existing records
    records->count = 0;
    
    char line[256];
    int lineNumber = 0;
    
    // Read file line-by-line. CSV format assumed: Name,RollNumber,Marks,Status
    while (fgets(line, sizeof(line), file)) {
        lineNumber++;
        
        // Skip header line
        if (lineNumber == 1) continue;
        
        // Remove newline character
        line[strcspn(line, "\n")] = '\0';
        
        // Parse CSV line
        char *token = strtok(line, ",");
        if (token == NULL) continue;
        
        if (records->count >= records->capacity) {
            records->capacity *= 2;
            records->students = (Student*)realloc(records->students, records->capacity * sizeof(Student));
            if (records->students == NULL) {
                printf("Memory allocation failed!\n");
                fclose(file);
                return;
            }
        }
        
        strcpy(records->students[records->count].name, token);
        
        token = strtok(NULL, ",");
        if (token == NULL) continue;
        records->students[records->count].rollNumber = atoi(token);
        
        token = strtok(NULL, ",");
        if (token == NULL) continue;
        records->students[records->count].marks = atof(token);
        
        records->count++;
    }
    
    fclose(file);
    printf("Student records loaded from '%s' successfully! (%d students loaded)\n", 
           filename, records->count);


    // If any valid records were loaded, display them immediately so the user can confirm contents right away.
    if (records->count > 0) {
        displayAllStudents(records);
    }
}


void clearInputBuffer() {
    int c;
    // Consume remaining chars up to newline so subsequent fgets() works.
    while ((c = getchar()) != '\n' && c != EOF);
}
```

---

## 6. Quick Code Explanation

This is a high-level Explanation the code  

- **Main structure:** The program defines a `Student` struct (name, roll number, marks) and a `StudentRecordSystem` struct that holds a dynamic array of students plus `count` and `capacity`.
- **Initialization:** `initializeSystem()` allocates an initial array of 100 students and makes sure memory allocation succeeds before the program continues.
- **User interface:** `main()` shows a text menu in a loop. Based on the user’s choice (1–11) it calls functions to add, display, search, modify, remove, sort, save, or load students, or to exit the program.
- **Adding and viewing students:** `addStudent()` collects validated input (name, unique positive roll number, marks between 0 and 100). `displayAllStudents()` prints a neat table and shows `PASS` or `FAIL` depending on whether marks are at least 40.
- **Searching and editing:** `searchStudent()` looks up a student by roll number and prints their details. `modifyStudent()` lets the user change name and/or marks while keeping other fields the same.
- **Deleting and statistics:** `removeStudent()` deletes a student by shifting later records left and reducing `count`. `calculateAverageMarks()` loops through all students and prints the class average.
- **Sorting:** A global flag `g_sortAscending` and the helper `compareMarks()` are used with `qsort()` so `sortStudents()` can sort by marks in either ascending or descending order, then immediately display the sorted list.
- **File operations:** `saveToFile()` writes all records as CSV (including computed pass/fail status). `loadFromFile()` reads CSV lines back, dynamically growing the array as needed and then displaying the loaded data.
- **Input safety:** `clearInputBuffer()` cleans up leftover characters after `scanf()`, and `fgets()` is used for strings to avoid buffer overflows and mixed input problems.

Overall, the code ties these pieces together into a simple command‑line system that safely manages student records in memory and on disk.

---

## 7. Key Features->Function Map and Edge Cases

### Key Features->Function Table

| Feature | Function |
|---------|----------|
| Add student with validation | `addStudent()` |
| Display formatted table | `displayAllStudents()` |
| Search by roll number | `searchStudent()` |
| Modify name or marks | `modifyStudent()` |
| Remove student | `removeStudent()` |
| Calculate average marks | `calculateAverageMarks()` |
| Sort ascending/descending | `sortStudents()` |
| Save to CSV | `saveToFile()` |
| Load from CSV | `loadFromFile()` |
| Dynamic array resizing | `realloc()` |
| Pass/fail status | `displayAllStudents()` |
| CLI menu loop | `main()` |

### Edge Cases Handled

1. **Empty Record Set** – All display/analyze functions check `if (count == 0)` and print friendly message
2. **Duplicate Roll Numbers** – `addStudent()` prevents duplicate roll numbers
3. **Array Capacity Exceeded** – `realloc()` automatically doubles capacity
4. **Invalid Input** – Marks validated 0–100, roll numbers > 0, names non-empty
5. **File I/O Errors** – `fopen()` checked; error message printed if file not found
6. **Malformed CSV** – `strtok()` validated; missing fields skipped
7. **Memory Allocation Failure** – `malloc()` checked; program exits with error message
8. **Partial Modification** – User can press Enter to skip a field
9. **Character Buffer Overflow** – `fgets()` used instead of `gets()` (fixed-size buffers)
10. **Input Buffer Pollution** – `clearInputBuffer()` called after `scanf()` to remove trailing newline

---

## 8. Conclusion

The **Student Record System** is a robust, modular C application that demonstrates core programming concepts: dynamic memory management, input validation, file I/O, sorting algorithms, and user interface design. 

### What We Learned

1. **Modular Design:** Separating concerns (CRUD, I/O, sorting, UI) makes code maintainable and testable.
2. **Error Handling:** Validating input and checking return values (malloc, fopen) prevents crashes.
3. **User Experience:** Clear prompts, formatted output, and graceful handling of edge cases build trust.
4. **Memory Safety:** Using `fgets()` over `gets()`, checking buffer sizes, and freeing memory prevents vulnerabilities.
5. **Standard Library Functions:** `qsort()`, `strtok()`, `realloc()` are powerful tools when used correctly.


### Future Enhancements

- **Sorting by name or roll number** (not just marks)
- **Grade statistics** (min, max, median, standard deviation)
- **Batch operations** (import 1000+ records from CSV)

