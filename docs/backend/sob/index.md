# SoB 账套 (WIP)

`SoB` (Set of Books) is the unit for accounting activities. Usually one `SoB` represents one company, or a department.

## Entities and Relations

``` mermaid
erDiagram
    SOB {
        UUID id
        string name
        string description
        string baseCurrency
        int startingPeriodYear
        int startingPeriodMonth
        int[] accountsCodeLength
    }
```
