Rhymix\Framework\Calendar
-------------------------

#### getMonthName()

```
public static function getMonthName(
    int $month_number,
    bool $long_format = true
): string
```

This method returns the English name of a month, e.g. 9 = 'September'.

#### getMonthStartDayOfWeek()

```
public static function getMonthStartDayOfWeek(
    int $month_number,
    ?int $year = null
): int
```

This method returns the day on which a month begins.
0 = Sunday, 1 = Monday, 2 = Tuesday, 3 = Wednesday, 4 = Thursday, 5 = Friday, 6 = Saturday.
If you do not specify a year, the current year is assumed.

#### getMonthDays()

```
public static function getMonthDays(
    int $month_number,
    ?int $year = null
): int
```

This method returns the number of days in a month, e.g. February 2016 has 29 days.
If you do not specify a year, the current year is assumed.
You must specify a year to get the number of days in February.

#### getMonthCalendar()

```
public static function getMonthCalendar(
    int $month_number,
    ?int $year = null,
    int $start_dow = 0
): array
```

This method returns a complete calendar for a month.
The return value is an array with six members, each representing a week.
Each week is an array with seven members, each representing a day.
6 weeks are returned. Empty cells are represented by nulls.
If you do not specify a year, the current year is assumed.
