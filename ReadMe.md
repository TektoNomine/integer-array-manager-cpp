# Structured Programming – Integer Array Manager

**Software Development Yr1 Group Project – Semester 2 2025**

---

This project is worth **30%** of your total marks for this module and is a team project.

Teams will consist of four students.

All members of each team are expected to make an equal contribution to the project and participate in all aspects of it, including problem development and analysis, coding, commenting of code, and documentation. Teams should work together when possible and class time is an ideal opportunity to do this. Individual marks will be awarded where it is apparent that team members have not made an equal contribution to the project.

> **Note:** Where it is obvious that a team member has made little or no contribution to the work effort of the project deliverables, this will result in a mark of zero.

The project solution and documentation must be uploaded to Moodle by Monday, **28/04/2025** no later than 5pm.

---

## Project Description

The project is based on how **Arrays** are handled in **C++**.
The application is a **menu-driven command-line program** that manipulates the contents of an **integer array**.

You can produce the code to meet the minimum requirements of the project, but **extra marks** are reserved for additional functionality, such as error trapping and handling, etc.

Marks will be awarded based on functionality, style, code commenting, and minimisation of the coding.
All aspects of the project should be presented professionally with strong attention to detail.

---

## Functional Requirements

The application must:

- Populate the array either by reading **12 integers from the keyboard** or by reading integers from a file **Numbers.dat**.
- Allow the user to perform the following operations via a menu:

### Menu Options

1. **Display:** Display the contents of the array.
2. **GetTotal:** Return the total of all numbers in the array.
3. **GetAverage:** Return the average value of the numbers (rounded to 2 decimal places).
4. **GetLargest:** Return the largest integer in the array.
5. **GetSmallest:** Return the smallest integer in the array.
6. **GetNumOccurrences:** Return how many times a particular number appears in the array.
7. **ScaleUp:** Multiply each element in the array by a user-entered factor.
8. **Reverse:** Reverse the order of the array elements (update the array, not just display reversed).
9. **ZeroBase:** Adjust all values so that the smallest number becomes zero.
10. **RemoveNumber:** Remove an element by its position (1st, 2nd, etc.).
11. **Sort:** Sort the array values in ascending order.
12. **Quit:** Save changes to the file and exit the program.

---

## Implementation Details

- Each menu option should call the appropriate **separate function**.
- Function prototypes and matching definitions must be written for all of the major functions.
- When the program starts, it should populate the array from the file **Numbers.dat**.
- When quitting, the program should save the current array content back into **Numbers.dat**.

---

## Extension Tasks

**Part A:**  
Develop the initial version of the application that manipulates a fixed-size array populated with 12 integers.

**Part B:**  
Adapt the application to allow reading **any number of integers** from the file **Numbers.dat**, **up to a maximum of 50 integers**.

