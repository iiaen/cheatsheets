Source: https://www.sqlbi.com/articles/using-generate-and-row-instead-of-addcolumns-in-dax/  with some additions
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
            "Year Month", YearMonthName,
            "IsWeekend", WEEKDAY( BaseDate, 2) IN {6,7} //optional
        )
    )
```

Useful Links
- https://www.daxformatter.com/
- https://community.powerbi.com/t5/Data-Stories-Gallery/bd-p/DataStoriesGallery
- https://github.com/PowerBi-Projects/PowerBI-visuals/
- https://docs.microsoft.com/en-us/power-bi/developer/visuals/utils-formatting
- https://powerbi.microsoft.com/en-us/blog/
