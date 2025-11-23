```c


// student_record_commented.c
//
// Fully commented version of the Student Record System.
// Comments explain each struct, function, block and the important lines.
// The original program logic is preserved; comments are added throughout
// to explain what's happening.

#include <stdio.h>  // standard I/O functions
#include <stdlib.h> // standard library functions (malloc, free, qsort, etc.)
#include <string.h> // string manipulation functions
#include <ctype.h>  // character type functions (e.g., isdigit)

/* Maximum length for a student's name (including terminating NUL). */
#define MAX_NAME_LENGTH 50

/* Marks threshold considered "passing". This is a program-wide constant. */
#define PASSING_MARKS 40

/*
 * Student
 * --------
 * Represents a single student's record stored in memory.
 * Fields:
 *  - name: fixed-size char array to hold the student's name (NUL-terminated).
 *  - rollNumber: unique integer identifier for the student.
 *  - marks: floating-point number representing the student's marks (0-100).
 */
typedef struct
{
    char name[MAX_NAME_LENGTH]; /* student's full name (NUL-terminated) */
    int rollNumber;             /* unique id for student */
    float marks;                /* marks scored (0.0 - 100.0) */
} Student;

/*
 * StudentRecordSystem
 * -------------------
 * Holds the entire collection of students and metadata about the array.
 * Fields:
 *  - students: dynamically allocated array of Student structs.
 *  - count: number of valid Student entries currently stored.
 *  - capacity: current allocated capacity (number of Student slots allocated).
 *
 * We use a dynamic array (heap allocated) and double the capacity when
 * more space is needed. This is a common pattern for simple resizable
 * containers in C.
 */
typedef struct
{
    Student *students; /* pointer to first element of dynamically allocated array */
    int count;         /* how many students are currently stored */
    int capacity;      /* how many students we can store before reallocating */
} StudentRecordSystem;

// -------------------------------------------------------------------------
// Function declarations - forward declarations so functions can be arranged
// in any order below without compile-time errors.
// -------------------------------------------------------------------------

/* Initialize a StudentRecordSystem structure and allocate initial storage. */
void initializeSystem(StudentRecordSystem *records);

/* Print the welcome banner shown at the program start. */
void displayWelcomeMessage();

/* Add a new student (reads data interactively from stdin). */
void addStudent(StudentRecordSystem *records);

/* Print a table of all students to stdout (read-only view). */
void displayAllStudents(const StudentRecordSystem *records);

/* Search for a student by roll number and print the student's details. */
void searchStudent(const StudentRecordSystem *records);

/* Modify an existing student's data (name and/or marks). */
void modifyStudent(StudentRecordSystem *records);

/* Remove a student by roll number. */
void removeStudent(StudentRecordSystem *records);

/* Compute and print the average marks across all students. */
void calculateAverageMarks(const StudentRecordSystem *records);

// Sort students by marks.
// params: records - pointer to the in-memory StudentRecordSystem
//         ascending - 1 to sort ascending, 0 to sort descending
void sortStudents(StudentRecordSystem *records, int ascending);

// Save student records to a CSV file (interactive filename input).
// params: records - pointer to the system to read students from
void saveToFile(const StudentRecordSystem *records);

// Load student records from a CSV file (interactive filename input).
// params: records - pointer to the system to populate with loaded students
void loadFromFile(StudentRecordSystem *records);

/* Print the interactive menu. */
void displayMenu();

/* Utility: read and discard remaining characters on stdin until newline/EOF. */
void clearInputBuffer();

/* Ask the user to confirm an action before it runs (returns 1 = yes, 0 = no). */
int confirmAction(const char *actionDescription);



// main — program entry point
// This function runs the interactive program loop. High-level behavior:
// 1) initialize the in-memory `records` container,
// 2) repeatedly show a menu and read the user's choice,
// 3) dispatch the choice to the appropriate function, and
// 4) ask whether to continue. Important "how" notes for newcomers:
// - scanf returns the number of items successfully parsed (e.g. 1 on success),
//   so we test the return value to detect invalid input.
// - after using scanf we call clearInputBuffer() to remove the leftover
//   newline so subsequent fgets calls behave as expected.
int main()
{
    /*
     * We name the local variable `records` to be explicit about what it holds.
     * This object will live for the lifetime of the program and holds the
     * dynamically allocated student array.
     */
    StudentRecordSystem records; /* on-stack container that holds pointers */

    /* Initialize the dynamic array and metadata (capacity, count). */
    initializeSystem(&records);

    /* Buffer to store the user's name when they start the program. */
    char userName[50];

    /* Greet the user. */
    displayWelcomeMessage();
    printf("\nPlease enter your name: ");

    /* Read the user's name. fgets is safer than gets because it bounds input. */
    fgets(userName, sizeof(userName), stdin); // read a line; includes trailing '\n' if it fits
    userName[strcspn(userName, "\n")] = '\0'; // strcspn returns index of first '\n'; we replace it with '\0' to remove newline

    printf("\nHello, %s! Welcome to the Student Record System.\n\n", userName);

    int choice; /* holds the menu selection */

    /* Main interactive loop: show menu, get choice, execute, ask whether to continue. */
    do
    {
        displayMenu(); /* print menu */
        printf("Enter your choice (1-11): ");

        /* Read an integer choice safely. How this works:
         * - scanf("%d", &choice) tries to parse an integer and returns how
         *   many items were successfully read (1 if an int was parsed).
         * - If the return is not 1, parsing failed (user typed non-digits),
         *   so we clear the input buffer and continue the loop.
         */
        if (scanf("%d", &choice) != 1)
        {
            printf("Invalid input! Please enter a number.\n");
            clearInputBuffer(); /* discard bad input (leftover chars) */
            continue;           /* go back to menu */
        }
        clearInputBuffer(); /* remove leftover newline after scanf */

        /* Dispatch based on the user's menu choice. We pass &records to each
         * function so they operate on the same in-memory container.
         * Before running any action (except Exit), we ask the user to confirm. */
        switch (choice)
        {
        case 1:
            if (!confirmAction("add a new student"))
                break;
            addStudent(&records);
            break;
        case 2:
            if (!confirmAction("display all students"))
                break;
            displayAllStudents(&records);
            break;
        case 3:
            if (!confirmAction("search for a student"))
                break;
            searchStudent(&records);
            break;
        case 4:
            if (!confirmAction("modify student records"))
                break;
            modifyStudent(&records);
            break;
        case 5:
            if (!confirmAction("remove a student"))
                break;
            removeStudent(&records);
            break;
        case 6:
            if (!confirmAction("calculate average marks"))
                break;
            calculateAverageMarks(&records);
            break;
        case 7:
            if (!confirmAction("sort students by marks in ascending order"))
                break;
            sortStudents(&records, 1); /* ascending */
            break;
        case 8:
            if (!confirmAction("sort students by marks in descending order"))
                break;
            sortStudents(&records, 0); /* descending */
            break;
        case 9:
            if (!confirmAction("save records to a file"))
                break;
            saveToFile(&records);
            break;
        case 10:
            if (!confirmAction("load records from a file"))
                break;
            loadFromFile(&records);
            break;
        case 11:
            /* Graceful exit path: user requested to quit. We still confirm before exiting. */
            if (confirmAction("exit the program"))
            {
                printf("Thank you for using the Student Record System, %s! Goodbye!\n", userName);
            }
            else
            {
                /* User cancelled exit; reset choice so the loop continues. */
                choice = 0;
            }
            break;
        default:
            printf("Invalid choice! Please try again.\n");
        }

        printf("\n"); /* spacing */

        /* After performing the chosen action, ask whether the user wants to
         * continue. This gives control to the user instead of looping
         * automatically. Keep the prompt minimal but clear.
         */
        if (choice != 11)
        {
            char contBuf[8]; /* small buffer for the reply */
            printf("Do you want to continue? (y/n): ");
            /* fgets reads a full line (up to size-1 chars) and includes the
             * trailing newline if space permits. It returns NULL on EOF/error.
             * We check the first character for 'n' or 'N' to mean "no".
             */
            if (fgets(contBuf, sizeof(contBuf), stdin) != NULL)
            {
                /* Only check the first character; accept 'n' or 'N' to exit. */
                if (contBuf[0] == 'n' || contBuf[0] == 'N')
                {
                    choice = 11; /* force loop termination */
                    printf("Exiting as requested.\n");
                }
            }
        }
    } while (choice != 11);

    /* Free heap-allocated data before exiting. This is good practice though
     * modern OSes will reclaim memory when the process ends. Explicit free
     * helps if this code is ever embedded in a long-running program or
     * library.
     */
    free(records.students);

    return 0; /* normal termination */
}

/* ------------------------------------------------------------------------- */
/* initializeSystem - allocate initial storage and set counters               */
/* ------------------------------------------------------------------------- */
void initializeSystem(StudentRecordSystem *records)
{
    /*
     * Choose a reasonable starting capacity. We allocate enough slots so the
     * program can hold a modest number of students without repeated reallocs.
     * Doubling strategy in addStudent keeps amortized realloc cost low.
     */
    records->capacity = 100; /* initial number of Student slots */

    /* Allocate memory on the heap for `capacity` Student elements.
     * We compute bytes as capacity * sizeof(Student). malloc returns a
     * pointer to the allocated region or NULL on failure.
     */
    records->students = (Student *)malloc(records->capacity * sizeof(Student)); // We allocate space for `capacity` Student structs (bytes = capacity * sizeof(Student))

    /* Start with zero students stored. */
    records->count = 0;

    /* Check allocation result to avoid dereferencing NULL later. */
    if (records->students == NULL)
    {
        printf("Memory allocation failed!\n");
        exit(1); /* unrecoverable in this small program */
    }
}

/* ------------------------------------------------------------------------- */
/* displayWelcomeMessage - prints a simple banner                           */
/* ------------------------------------------------------------------------- */
void displayWelcomeMessage()
{
    printf("========================================\n");
    printf("     STUDENT RECORD SYSTEM\n");
    printf("========================================\n");
}

/* ------------------------------------------------------------------------- */
/* displayMenu - prints the available menu options                          */
/* ------------------------------------------------------------------------- */
void displayMenu()
{
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

m

/* ------------------------------------------------------------------------- */
/* displayAllStudents - print the stored students in a human-friendly table  */
/* ------------------------------------------------------------------------- */
void displayAllStudents(const StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    printf("\n--- All Students ---\n");
    /* We print a fixed-width header so columns align. See the format below. */
    printf("%-5s %-20s %-12s %-8s %s\n", "S.No", "Name", "Roll Number", "Marks", "Status"); // %-5s means left-align string in width 5
    printf("------------------------------------------------------------\n");

    for (int i = 0; i < records->count; i++)
    {
        const Student *student = &records->students[i];

        /* Determine pass/fail status from marks. */
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL"); // copy PASS/FAIL into status (strcpy expects NUL-terminated source)

        /*
         * Row format string explanation:
         * - %-5d   : left-align integer in width 5 (serial number)
         * - %-20s  : left-align string in width 20 (student->name)
         * - %-12d  : left-align integer in width 12 (roll number)
         * - %-8.2f : left-align float in width 8 with 2 decimals (marks)
         * - %s     : status string
         * The following printf supplies these values in the same order.
         */
        printf("%-5d %-20s %-12d %-8.2f %s\n",
               i + 1, student->name, student->rollNumber, student->marks, status); // fields: serial, name, roll, marks (2 decimals), status
    }
}

/* ------------------------------------------------------------------------- */
/* searchStudent - find a student by roll number and print details           */
/* ------------------------------------------------------------------------- */
void searchStudent(const StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    int rollNumber;
    printf("\n--- Search Student ---\n");
    printf("Enter roll number to search: ");

    if (scanf("%d", &rollNumber) != 1)
    {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();

    /* Linear search - O(n) but simplest for moderate sizes. */
    for (int i = 0; i < records->count; i++)
    {
        if (records->students[i].rollNumber == rollNumber)
        {
            const Student *student = &records->students[i];
            char status[10];
            strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");

            printf("\nStudent Found:\n");
            printf("Name: %s\n", student->name);
            printf("Roll Number: %d\n", student->rollNumber);
            printf("Marks: %.2f\n", student->marks);
            printf("Status: %s\n", status);
            return; /* found - return early */
        }
    }

    printf("Student with roll number %d not found.\n", rollNumber);
}

/* ------------------------------------------------------------------------- */
/* modifyStudent - change name or marks for an existing student             */
/* ------------------------------------------------------------------------- */
void modifyStudent(StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    int rollNumber;
    printf("\n--- Modify Student Record ---\n");
    printf("Enter roll number of student to modify: ");

    if (scanf("%d", &rollNumber) != 1)
    {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();

    /* Find the student to modify. */
    for (int i = 0; i < records->count; i++)
    {
        if (records->students[i].rollNumber == rollNumber)
        {
            printf("Student found. Current details:\n");
            printf("Name: %s\n", records->students[i].name);
            printf("Roll Number: %d\n", records->students[i].rollNumber);
            printf("Marks: %.2f\n", records->students[i].marks);

            /* Allow user to change the name; pressing Enter keeps current. */
            printf("\nEnter new name (or press Enter to keep current): ");
            char newName[MAX_NAME_LENGTH];
            fgets(newName, sizeof(newName), stdin);
            newName[strcspn(newName, "\n")] = '\0';

            if (strlen(newName) > 0)
            {
                /* Replace the name only if user typed something. */
                strcpy(records->students[i].name, newName);
            }

            /* Allow user to change marks; pressing Enter keeps current. */
            printf("Enter new marks (or press Enter to keep current): ");
            char marksInput[32];
            if (fgets(marksInput, sizeof(marksInput), stdin) != NULL)
            {
                marksInput[strcspn(marksInput, "\n")] = '\0';
                if (strlen(marksInput) == 0)
                {
                    printf("No marks entered; keeping current marks.\n");
                }
                else
                {
                    char *endptr;
                    float newMarks = strtof(marksInput, &endptr);
                    /* Validate numeric input and range. */
                    if (endptr != marksInput && *endptr == '\0')
                    {
                        if (newMarks >= 0 && newMarks <= 100)
                        {
                            records->students[i].marks = newMarks;
                        }
                        else
                        {
                            printf("Invalid marks! Keeping current marks.\n");
                        }
                    }
                    else
                    {
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

/* ------------------------------------------------------------------------- */
/* removeStudent - delete a student by shifting subsequent elements left     */
/* ------------------------------------------------------------------------- */
void removeStudent(StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    int rollNumber;
    printf("\n--- Remove Student ---\n");
    printf("Enter roll number of student to remove: ");

    if (scanf("%d", &rollNumber) != 1)
    {
        printf("Invalid input!\n");
        clearInputBuffer();
        return;
    }
    clearInputBuffer();

    for (int i = 0; i < records->count; i++)
    {
        if (records->students[i].rollNumber == rollNumber)
        {
            /* Shift elements left to overwrite the removed entry. */
            for (int j = i; j < records->count - 1; j++)
            {
                records->students[j] = records->students[j + 1];
            }
            records->count--; /* logically remove the last duplicate element */
            printf("Student with roll number %d removed successfully!\n", rollNumber);
            return;
        }
    }

    printf("Student with roll number %d not found.\n", rollNumber);
}

/* ------------------------------------------------------------------------- */
/* calculateAverageMarks - simple aggregate computation                      */
/* ------------------------------------------------------------------------- */
void calculateAverageMarks(const StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    float total = 0;
    for (int i = 0; i < records->count; i++)
    {
        total += records->students[i].marks; /* accumulate marks */
    }

    float average = total / records->count; /* compute mean */
    printf("\n--- Average Marks ---\n");
    printf("Total students: %d\n", records->count);
    printf("Average marks: %.2f\n", average);
}

// -------------------------------------------------------------------------
// Global helper used by the qsort comparator below to switch direction.
// We keep this file-scoped so the comparator can behave for ascending/descending.
// -------------------------------------------------------------------------
static int g_sortAscending = 1; /* 1 = ascending, 0 = descending */

/*
 * compareMarks
 * -------------
 * Comparator for qsort. qsort requires a function that compares two elements
 * and returns negative/zero/positive depending on ordering. We consult the
 * global flag g_sortAscending to change the order.
 */
// compareMarks - comparator used by qsort
// params: a, b - pointers to Student objects (passed as void* by qsort)
// uses global g_sortAscending to decide direction
static int compareMarks(const void *a, const void *b)
{
   
    //
    //  `a` and `b` are pointers to elements in the  array (they are NOT Student values).
   //  After the cast, `sa` and `sb` are pointers to the two Student elements qsort wants us to compare.
    const Student *sa = (const Student *)a; // 'a' -> pointer to first Student element
    const Student *sb = (const Student *)b; // 'b' -> pointer to second Student element

    /* Compare marks and return order depending on g_sortAscending. */
    if (sa->marks < sb->marks)
        return g_sortAscending ? -1 : 1; // a < b -> negative for ascending, positive for descending
    if (sa->marks > sb->marks)
        return g_sortAscending ? 1 : -1; // a > b -> positive for ascending, negative for descending
    return 0;                            // equal marks -> comparator returns 0 (no ordering)
}

/* ------------------------------------------------------------------------- */
/* sortStudents - wrapper that sets comparator direction then calls qsort     */
/* ------------------------------------------------------------------------- */
void sortStudents(StudentRecordSystem *records, int ascending)
{
    if (records->count == 0)
    {
        printf("No students in the record system.\n");
        return;
    }

    /* Configure comparator direction and call qsort. qsort is an in-place
     * sort which rearranges the elements in the array. It is typically fast
     * (O(n log n) average) and concise to use compared to hand-rolled sorts.
     */
    g_sortAscending = ascending ? 1 : 0;

    /* We call qsort with:
     * - pointer to first element
     * - number of elements
     * - size of each element in bytes
     * - comparator function
     * qsort will call compareMarks repeatedly to order the elements.
     */
    qsort(records->students, records->count, sizeof(Student), compareMarks); // We ask the C library to sort our array in-place

    printf("Students sorted by marks in %s order.\n", ascending ? "ascending" : "descending");
    displayAllStudents(records); /* show results immediately */
}

/* ------------------------------------------------------------------------- */
i/* ------------------------------------------------------------------------- */
void saveToFile(const StudentRecordSystem *records)
{
    if (records->count == 0)
    {
        printf("No students to save.\n");
        return;
    }

    char filename[100];
    printf("\n--- Save to File ---\n");
    printf("Enter filename to save records: ");
    // Read filename with fgets into bounded buffer. fgets may include '\n'.
    fgets(filename, sizeof(filename), stdin); // read filename (bounded)
    // strcspn returns the index of the first occurrence of any char in the
    // second argument (here '\n'). We replace it with '\0' to strip newline.
    filename[strcspn(filename, "\n")] = '\0'; // remove trailing newline so fopen gets a clean name

    // fopen opens the file; mode "w" means create/truncate for writing.
    FILE *file = fopen(filename, "w");
    if (file == NULL)
    {
        // fopen returns NULL on failure (e.g., permission denied). Always check!
        printf("Error opening file for writing!\n");
        return;
    }

    /* Print CSV header and rows. Status is derived from marks. */
    fprintf(file, "Name,RollNumber,Marks,Status\n");
    for (int i = 0; i < records->count; i++)
    {
        const Student *student = &records->students[i];
        char status[10];
        strcpy(status, (student->marks >= PASSING_MARKS) ? "PASS" : "FAIL");
        fprintf(file, "%s,%d,%.2f,%s\n",
                student->name, student->rollNumber, student->marks, status);
    }

    // flush and close the file when done
    fclose(file); // close the file and release resources
    printf("Student records saved to '%s' successfully!\n", filename);
}

/* ------------------------------------------------------------------------- */
/* loadFromFile - read CSV file and populate the in-memory array             */
/* ------------------------------------------------------------------------- */
void loadFromFile(StudentRecordSystem *records)
{
    char filename[100];
    printf("\n--- Load from File ---\n");
    printf("Enter filename to load records: ");
    fgets(filename, sizeof(filename), stdin); // read filename (bounded)
    // strcspn finds newline position; replace it with '\0' to trim
    filename[strcspn(filename, "\n")] = '\0'; // remove trailing newline

    // fopen with mode "r" opens file for reading. Check for NULL in case the file
    // doesn't exist or we lack permissions.
    FILE *file = fopen(filename, "r");
    if (file == NULL)
    {
        printf("Error opening file for reading!\n");
        return;
    }

    /* Reset count to clear current records in memory. We keep capacity so we
     * can reuse allocated space and avoid realloc if file has fewer records.
     */
    records->count = 0;

    char line[256];
    int lineNumber = 0;

    // Read lines from file using fgets (safe bounded read).
    // Loop continues while fgets() returns a non-NULL pointer (a line was read).
    // It stops when fgets() returns NULL (EOF reached or read error).
    while (fgets(line, sizeof(line), file))
    {
        lineNumber++;

        /* Skip header row (assumed to be first). */
        if (lineNumber == 1)
            continue;

        /*
         * Strip newline and parse CSV tokens. Notes for readers:
         * - strcspn finds the position of '\n' and we overwrite it with '\0'.
         * - strtok modifies the input string and returns a pointer to the
         *   first token (fields separated by ','). It returns NULL on error
         *   or when no token is found. This implementation assumes fields
         *   do not contain embedded commas or quotes.
        */
        line[strcspn(line, "\n")] = '\0'; // trim newline from the CSV line
        char *token = strtok(line, ",");  // get first CSV token (name) by splitting on ','
        if (token == NULL)
            continue; // if no token, skip this malformed line

    // Grow array if necessary.
        if (records->count >= records->capacity)
        {
            records->capacity *= 2;
            records->students = (Student *)realloc(records->students, records->capacity * sizeof(Student));
            if (records->students == NULL)
            {
                printf("Memory allocation failed!\n");
                fclose(file);
                return;
            }
        }

    // token now points to the first CSV field (name). Copy into struct.
    // Note: strcpy copies the NUL-terminated string; ensure token length < MAX_NAME_LENGTH
    strcpy(records->students[records->count].name, token); // copy name token into student's name field

        // Next field: roll number.
        // - strtok(NULL, ",") returns the next token from the same string.
        // - atoi converts the token string to an int (returns 0 for
        //   validate, but atoi is concise and sufficient here.
        token = strtok(NULL, ","); // next token is roll number (string)
        if (token == NULL)
            continue; // malformed line if missing
        records->students[records->count].rollNumber = atoi(token); // convert roll number string to int (atoi)

    // Next field: marks.
        token = strtok(NULL, ",");
        if (token == NULL)
            continue;
        records->students[records->count].marks = atof(token);

        records->count++;
    }

    // close the input file when finished reading
    fclose(file);
    printf("Student records loaded from '%s' successfully! (%d students loaded)\n",
           filename, records->count);

    /* Display loaded records immediately so the user can confirm contents. */
    if (records->count > 0)
    {
        displayAllStudents(records);
    }
}

/* ------------------------------------------------------------------------- */
/* confirmAction - ask user to confirm an operation before running it        */
/* Returns 1 if user confirms with y/Y, otherwise returns 0 and cancels.     */
/* ------------------------------------------------------------------------- */
int confirmAction(const char *actionDescription)
{
    char response[8];
    printf("Are you sure you want to %s? Press y/Y to continue, n/N to cancel: ", actionDescription);
    if (fgets(response, sizeof(response), stdin) == NULL)
    {
        printf("Input error. Cancelling action.\n");
        return 0;
    }

    if (response[0] == 'y' || response[0] == 'Y')
    {
        return 1;
    }

    printf("Action cancelled.\n");
    return 0;
}

/* ------------------------------------------------------------------------- */
/* clearInputBuffer - consume characters until newline/EOF                   */
/* This prevents leftover input (e.g., after scanf) from affecting later reads. */
/* ------------------------------------------------------------------------- */
void clearInputBuffer()
{
    int c;
    while ((c = getchar()) != '\n' && c != EOF)
        ;
}


```