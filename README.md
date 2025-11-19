# PL-SQL_Bonus_Management


### Student: SADE GEORGE SADE
### Student ID: 26915
### Course: PL/SQL (INSY 8311)
### Instructor: Eric Maniraguha

# Problem Definition
## Problem Title: Employee Bonus Management System

## A company needs a system to calculate bonuses for employees. The system should:

- Store multiple employee salary values for processing (Collections)

- Keep employee information together (Records)

- Skip employees with problems like zero salary (GOTO)

- The system will calculate bonuses based on employee performance ratings and salaries.

# Objectives
- Create tables for employees and bonus records

- Use PL/SQL with Collections, Records, and GOTO

- Calculate bonuses for good performers

- Handle errors when data is bad

- Show results clearly
# TABLE SCRIPT
```SQL
CREATE TABLE my_employees (
    emp_id NUMBER PRIMARY KEY,
    name VARCHAR2(50),
    salary NUMBER,
    rating VARCHAR2(5)
);

-- bonus table
CREATE TABLE my_bonus_log (
    emp_id NUMBER,
    bonus NUMBER
);

-- Data for testing
INSERT INTO my_employees VALUES (1, 'John', 50000, 'A');
INSERT INTO my_employees VALUES (2, 'Sarah', 0, 'B');      -- Zero salary
INSERT INTO my_employees VALUES (3, 'Mike', 60000, 'A');

COMMIT;
```

# PROCEDURES
## PROCEDURE SCRIPT
```SQL
-- Collection 
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE demo_collections IS
    TYPE salary_list IS VARRAY(3) OF NUMBER;
    salaries salary_list := salary_list(50000, 45000, 60000);
    total NUMBER := 0;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== COLLECTIONS ===');
    FOR i IN 1..salaries.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE('Salary ' || i || ': $' || salaries(i));
        total := total + salaries(i);
    END LOOP;
    DBMS_OUTPUT.PUT_LINE('Total: $' || total);
END;
/

EXEC demo_collections;

-- Records
CREATE OR REPLACE PROCEDURE demo_records IS
    TYPE emp_rec IS RECORD (
        id NUMBER,
        name VARCHAR2(50),
        salary NUMBER
    );
    employee emp_rec;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== RECORDS ===');
    employee.id := 1;
    employee.name := 'John';
    employee.salary := 50000;
    DBMS_OUTPUT.PUT_LINE('ID: ' || employee.id);
    DBMS_OUTPUT.PUT_LINE('Name: ' || employee.name);
    DBMS_OUTPUT.PUT_LINE('Salary: $' || employee.salary);
END;
/

EXEC demo_records;

-- Goto Statements
CREATE OR REPLACE PROCEDURE demo_goto IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== GOTO ===');
    FOR e IN (SELECT * FROM my_employees) LOOP
        IF e.salary = 0 THEN
            DBMS_OUTPUT.PUT_LINE('Skipping ' || e.name || ' - zero salary');
            GOTO next_emp;
        END IF;
        DBMS_OUTPUT.PUT_LINE('Processing ' || e.name || ' - Salary: $' || e.salary);
        INSERT INTO my_bonus_log VALUES (e.emp_id, e.salary * 0.10);
        <<next_emp>>
        NULL;
    END LOOP;
    COMMIT;
END;
/

EXEC demo_goto;
```

##  Procedure 1 - Collections

<img width="951" height="531" alt="collections" src="https://github.com/user-attachments/assets/2583fb28-9872-4cf4-a698-2cc2a89427fb" />

## Procedure 2 - Records

<img width="959" height="539" alt="records" src="https://github.com/user-attachments/assets/ba7960d9-672c-4005-ab38-cb3499454215" />

## Procedure 3 - GOTO

<img width="959" height="533" alt="goto" src="https://github.com/user-attachments/assets/96b907b4-ed9d-43ff-a743-0420234b52b4" />


# Outcome
- The procedures worked successfully:

- Calculated employee bonuses and totals using a VARRAY collection

- Displayed employee information neatly using a Record structure

- Skipped invalid employees with zero salary using GOTO for clean processing








