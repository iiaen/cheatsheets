Source: https://www.sqlbi.com/articles/using-generate-and-row-instead-of-addcolumns-in-dax/  
What: favourite date table for PBI
```
01DateTable =
VAR BaseCalendar =
    CALENDAR ( DATE ( 2016, 1, 1 ), DATE ( 2018, 12, 31 ) ) //to change to min() max() of relevant date field
RETURN
    GENERATE (
        BaseCalendar,
        VAR BaseDate = [Date]
        VAR YearDate = YEAR ( BaseDate )
        VAR MonthNumber = MONTH ( BaseDate )
        VAR MonthName = FORMAT ( BaseDate, "mmmm" )
        VAR YearMonthName = FORMAT ( BaseDate, "mmm yy" )
        VAR YearMonthNumber = YearDate * 12 + MonthNumber - 1
        RETURN ROW (
            "Day", BaseDate,
            "Year", YearDate,
            "Month Number", MonthNumber,
            "Month", MonthName,
            "Year Month Number", YearMonthNumber,
            "Year Month", YearMonthName
        )
    )
```
