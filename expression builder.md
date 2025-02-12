# Azure Data Factory (ADF) Expression Builder - Employee Table Examples

## Employee Table Schema
| EmployeeID | Name    | Age | Department | Salary  | JoiningDate  |
|-----------|--------|----|------------|--------|-------------|
| 101       | Alice  | 30 | HR         | 50000  | 2020-01-15  |
| 102       | Bob    | 40 | IT         | 70000  | 2018-06-23  |
| 103       | Charlie| 28 | Finance    | 60000  | 2021-09-30  |
| 104       | David  | 35 | IT         | 75000  | 2017-11-12  |
| 105       | Eve    | 27 | HR         | 55000  | 2022-02-01  |

---

## 1. String Functions (Employee Name & Department)

```adf
toUpper(Name)                          -- Convert employee name to uppercase
toLower(Department)                    -- Convert department name to lowercase
length(Name)                            -- Get length of employee name
concat(Name, ' -
