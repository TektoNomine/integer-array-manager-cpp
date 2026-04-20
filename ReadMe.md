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

// --- ПИНЫ ---

// ЛЕВАЯ СТОРОНА (A)
const int A_GREEN = 4;
const int A_YELLOW = 3;
const int A_RED = 2;
const int BUTTON_A = 9;

// ПРАВАЯ СТОРОНА (B)
const int B_GREEN = 11;
const int B_YELLOW = 12;
const int B_RED = 13;
const int BUTTON_B = 5;

// ПЕШЕХОД
const int PED_BUTTON = 10;
const int PED_LED = 7;

// ДАТЧИК СВЕТА
const int LDR_PIN = A0;

// --- НАСТРОЙКИ ---
const int NIGHT_THRESHOLD = 2;

const unsigned long MIN_GREEN = 2000;

const unsigned long A_DAY = 10000;
const unsigned long A_NIGHT = 30000;

const unsigned long B_DAY = 10000;
const unsigned long B_NIGHT = 20000;

const unsigned long YELLOW = 2000;
const unsigned long ALL_RED = 2000;

const unsigned long PED_TOTAL = 5000;
const unsigned long PED_FLASH = 4000;

// --- SETUP ---
void setup() {
  pinMode(A_GREEN, OUTPUT);
  pinMode(A_YELLOW, OUTPUT);
  pinMode(A_RED, OUTPUT);

  pinMode(B_GREEN, OUTPUT);
  pinMode(B_YELLOW, OUTPUT);
  pinMode(B_RED, OUTPUT);

  pinMode(PED_LED, OUTPUT);

  pinMode(BUTTON_A, INPUT);
  pinMode(BUTTON_B, INPUT);
  pinMode(PED_BUTTON, INPUT);
}

// --- УСТАНОВКА СВЕТОФОРА ---
void setLights(bool ag, bool ay, bool ar,
               bool bg, bool by, bool br) {

  digitalWrite(A_GREEN, ag);
  digitalWrite(A_YELLOW, ay);
  digitalWrite(A_RED, ar);

  digitalWrite(B_GREEN, bg);
  digitalWrite(B_YELLOW, by);
  digitalWrite(B_RED, br);
}

// --- ДЕНЬ/НОЧЬ ---
bool isNight() {
  return analogRead(LDR_PIN) <= NIGHT_THRESHOLD;
}

// --- ПЕШЕХОД ---
bool pedPressed() {
  return digitalRead(PED_BUTTON) == HIGH;
}

void pedestrian() {
  unsigned long start = millis();
  unsigned long last = 0;
  bool state = false;

  // оба красные
  setLights(0,0,1, 0,0,1);

  while (millis() - start < PED_TOTAL) {

    if (millis() - start < PED_FLASH) {
      if (millis() - last > 300) {
        last = millis();
        state = !state;
        digitalWrite(PED_LED, state);
      }
    } else {
      digitalWrite(PED_LED, LOW);
    }

    delay(20);
  }

  digitalWrite(PED_LED, LOW);
}

// --- ОЖИДАНИЕ ---
void waitTime(unsigned long t) {
  unsigned long start = millis();

  while (millis() - start < t) {
    if (pedPressed()) {
      pedestrian();
      start = millis();
    }
    delay(20);
  }
}

// --- ЗЕЛЕНЫЙ С ЛОГИКОЙ ---
void waitGreen(bool forA, unsigned long maxT) {
  unsigned long start = millis();

  while (millis() - start < maxT) {

    int a = digitalRead(BUTTON_A);
    int b = digitalRead(BUTTON_B);

    if (pedPressed()) {
      pedestrian();

      if (forA)
        setLights(1,0,0, 0,0,1);
      else
        setLights(0,0,1, 1,0,0);

      start = millis();
    }

    if (millis() - start > MIN_GREEN) {
      if (forA && a==0 && b==1) break;
      if (!forA && b==0 && a==1) break;
    }

    delay(50);
  }
}

// --- LOOP ---
void loop() {

  unsigned long aMax = isNight() ? A_NIGHT : A_DAY;
  unsigned long bMax = isNight() ? B_NIGHT : B_DAY;

  // 1
  setLights(1,0,0, 0,0,1);
  waitGreen(true, aMax);

  // 2
  setLights(0,1,0, 0,0,1);
  waitTime(YELLOW);

  // 3
  setLights(0,0,1, 0,0,1);
  waitTime(ALL_RED);

  // 4
  setLights(0,0,1, 1,0,0);
  waitGreen(false, bMax);

  // 5
  setLights(0,0,1, 0,1,0);
  waitTime(YELLOW);

  // 6
  setLights(0,0,1, 0,0,1);
  waitTime(ALL_RED);
}