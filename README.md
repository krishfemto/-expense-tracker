# Expense Tracker

**Languages:** [English](README.md) | [日本語](README.jp.md)

---

A simple expense tracker with a category breakdown chart, built with Spring Boot + Thymeleaf.
Log expenses, see a running total, and view a pie chart of spending by category.

## What this is

After building a URL shortener as a first "junior Java developer" project, I wanted
a second one that involves slightly different skills — aggregating data (totals by
category) and visualizing it with a chart, alongside the same CRUD basics.

## How it works

![Architecture Diagram](architecture-diagram.png)

1. The homepage shows a form to add an expense (description, amount, category, date)
2. On submit, the expense is saved to the database
3. The page re-renders showing the updated expense list, total spending, and a
   pie chart breaking down spending by category
4. Each expense can be deleted from the list

## Tech used

- Java 17
- Spring Boot 3.2 (Web + Thymeleaf + JPA)
- H2 (in-memory database — no separate database server needed)
- Chart.js for the pie chart
- Jakarta Validation for form input

## Screenshot

![Expense Tracker dashboard](screenshot.png)

## Running it

```bash
mvn spring-boot:run
```

Then open `http://localhost:8080`.

The database is in-memory (H2), so data resets each time the app restarts.
You can browse it directly at `http://localhost:8080/h2-console`
(JDBC URL: `jdbc:h2:mem:expensetracker`, user: `sa`, no password).

## A few notes on how it's built

- **Category totals** are calculated with a single JPA query using `GROUP BY`
  and `SUM`, returned as a list of `(category, total)` pairs via a projection
  interface — rather than loading all expenses and summing them in Java.

- **The chart** is rendered with Chart.js. The category totals are passed from
  the controller to the template, then serialized into JavaScript arrays using
  Thymeleaf's `th:inline="javascript"` — so the chart always reflects the
  current data with no extra API call needed.

## Author

Krish — [GitHub](https://github.com/krishfemto)
