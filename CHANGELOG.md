# Change Log

## Student Record System

This version improves the original Student Record System in several key ways. The sorting logic was refactored from a manual bubble sort to the standard library `qsort` with a dedicated comparator, making sorting by marks more efficient and easier to maintain. User input handling was made more robust, especially in `modifyStudent`, where marks are now read as a line of text and validated, allowing the user to simply press Enter to keep existing marks instead of using a magic `-1` value. The initialization of the dynamic array now starts with a larger default capacity to reduce reallocations, and the main menu loop includes an additional confirmation prompt so users can exit quickly by replying `n`/`N` after any operation. Loading from file now automatically displays the loaded records so users can immediately verify the data. Throughout the code, comments and messages were clarified to better explain behavior and improve overall usability.
