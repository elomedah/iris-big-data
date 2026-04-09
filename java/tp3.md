# TP3 — Java: Control Flow, Methods, Arrays, and ArrayList

## 🎯 Objectives

This practical session covers:

- Control flow: `if/else`, `switch`, `while`, `for`
- Methods: definition, call, return, overloading
- Arrays: declaration, access, enhanced `for`
- `ArrayList`: creation, population, manipulation

---

#  Part 1 — Control Flow

## Exercise 1 — Even or Odd

Write a program that:
- asks the user for an integer
- displays "Even" or "Odd"

---

## Exercise 2 — Grade Evaluation

Input: mark (0–20)

Output:
- < 10 → Fail  
- 10–13 → Pass  
- 14–15 → Good  
- >= 16 → Very Good  

---

## Exercise 3 — Day of Week (switch)

Input: number (1–7)

Output:
- 1 → Monday  
- 2 → Tuesday  
- …  
- 7 → Sunday  
- Otherwise → Invalid  

---

## Exercise 4 — Sum from 1 to n

- Ask user for n
- Compute sum using:
  - while
  - for

---

## Exercise 5 — Multiplication Table

Display table of n from 1 to 10.

---

#  Part 2 — Methods

## Exercise 6 — Simple Method

```java
static void displayLine()
```

Print a separator line.

Call it 3 times.

---

## Exercise 7 — Method with Return

```java
static int square(int x)
```

Return square of a number.

---

## Exercise 8 — Max Function

```java
static int max(int a, int b)
```

Return the maximum.

---

## Exercise 9 — Method Overloading

Create:

```java
sum(int a, int b)
sum(int a, int b, int c)
sum(double a, double b)
```

---

#  Part 3 — Arrays

## Exercise 10 — Basic Array

```java
int[] values = {4, 8, 1, 9, 3};
```

Display:
- first element  
- last element  
- all elements  

---

## Exercise 11 — Sum and Average

Compute:
- sum  
- average  

Use for-each.

---

## Exercise 12 — Search

- Ask user for value  
- Check if exists in array  

---

## Exercise 13 — Min and Max

Find:
- minimum  
- maximum  

---

#  Part 4 — ArrayList

## Exercise 14 — Create List

```java
ArrayList<Integer> list = new ArrayList<>();
```

Add 5 values and display them.

---

## Exercise 15 — Sum of List

Compute sum of elements.

---

## Exercise 16 — Search and Remove

- Store names in ArrayList<String>
- Ask user for a name
- Remove it if found

---

#  Part 5 — Final Exercise

## Student Grades Manager

### Requirements:

1. Store marks in ArrayList<Integer>
2. Ask user for number of marks
3. Input marks using for
4. Create methods:

```java
static int max(ArrayList<Integer> list)
static int min(ArrayList<Integer> list)
static double average(ArrayList<Integer> list)
static void displayResult(int mark)
```

### Output:
- All marks  
- Min / Max / Average  
- Grade classification  
- Search for a mark  

---
