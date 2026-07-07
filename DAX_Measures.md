# DAX Measures

Documentation of all DAX measures used in the Academic Performance Dashboard.

---

## Grade Averages

### Average Grade
Average grade value across all grade types (ignores any grade type filter).

```dax
Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    ALL(grades[grade_type])
)
```

### Exam Average Grade
Average grade value filtered to `grade_type = "exam"`.

```dax
Exam Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    grades[grade_type] = "exam"
)
```

### Homework Average Grade
Average grade value filtered to `grade_type = "homework"`.

```dax
Homework Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    grades[grade_type] = "homework"
)
```

### Project Average Grade
Average grade value filtered to `grade_type = "project"`.

```dax
Project Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    grades[grade_type] = "project"
)
```

### Quiz Average Grade
Average grade value filtered to `grade_type = "quiz"`.

```dax
Quiz Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    grades[grade_type] = "quiz"
)
```

### Test Average Grade
Average grade value filtered to `grade_type = "test"`.

```dax
Test Average Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    grades[grade_type] = "test"
)
```

---

## Grade Counts

### All Type Grades
Count of all grade rows, ignoring any grade type filter.

```dax
All Type Grades = 
CALCULATE(
    COUNTROWS(grades),
    ALL(grades[grade_type])
)
```

### Total Grades
Total count of grade rows in the current filter context.

```dax
Total Grades = 
CALCULATE(
    COUNTROWS(grades)
)
```

### Global Grades Count
Total count of grade rows, ignoring filters on grade type, academic year, and department.

```dax
Global Grades Count = 
CALCULATE(
    COUNTROWS(
        grades
    ),
    ALL(grades[grade_type]),
    ALL(periods[academic_year]),
    ALL(subjects[department_hint])
)
```

### Neg Grades Count
Count of grades with a value below 6.

```dax
Neg Grades Count = 
CALCULATE(
    COUNTROWS(grades),
    grades[grade_value] < 6
)
```

### Neg Grades Prop
Proportion of negative grades (below 6) out of total grades.

```dax
Neg Grades Prop = 
DIVIDE([Neg Grades Count], [Total Grades], 0)
```

---

## Time Intelligence

### Previous Grade
Average grade value for the previous calendar month.

```dax
Previous Grade = 
CALCULATE(
    AVERAGE(grades[grade_value]),
    PREVIOUSMONTH('Calendar'[Date])
)
```

### MoM_AG%
Month-over-month percentage change in Average Grade, using the `Previous Grade` measure.

```dax
MoM_AG% = 
DIVIDE(
   ([Average Grade] - [Previous Grade]),
   [Previous Grade], 0
)
```

### Month-over-Month
Month-over-month percentage change in Average Grade, calculated inline with `DATEADD`.

```dax
Month-over-Month = 
VAR __PREV_MONTH = CALCULATE([Average Grade], DATEADD('Calendar'[Date], -1, MONTH))
RETURN
    DIVIDE([Average Grade] - __PREV_MONTH, __PREV_MONTH)
```

### YTD
Year-to-date Average Grade using the built-in `TOTALYTD` function.

```dax
YTD = 
TOTALYTD([Average Grade], 'Calendar'[Date])
```

### YTD_Manual
Year-to-date Average Grade using the built-in `TOTALYTD` function.

```dax
YTD_Manual = 
TOTALYTD(
    [Average Grade],
    calendar[Date]
)
```

---

## Table Reference

| Measure | Category | Depends On |
|---|---|---|
| Average Grade | Average | grades |
| Exam Average Grade | Average | grades |
| Homework Average Grade | Average | grades |
| Project Average Grade | Average | grades |
| Quiz Average Grade | Average | grades |
| Test Average Grade | Average | grades |
| All Type Grades | Count | grades |
| Total Grades | Count | grades |
| Global Grades Count | Count | grades, periods, subjects |
| Neg Grades Count | Count | grades |
| Neg Grades Prop | Count | Neg Grades Count, Total Grades |
| Previous Grade | Time Intelligence | grades, Calendar |
| MoM_AG% | Time Intelligence | Average Grade, Previous Grade |
| Month-over-Month | Time Intelligence | Average Grade, Calendar |
| YTD | Time Intelligence | Average Grade, Calendar |
| YTD_Manual | Time Intelligence | Average Grade, calendar |
