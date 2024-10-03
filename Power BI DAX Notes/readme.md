# Primary Objective

*My main goal in creating this repository is to demonstrate my understanding of DAX language which is used for creating measure and Columns. 
These are individually curated notes which I generally refer to while working with DAX langauge*

# DAX Functions and Usage

## ALL
```ALL([TableNameOrColumnName], [ColumnName1], ...)```  
Returns all the rows in a table, or all the values in a column, ignoring any filters that might have been applied.

Examples:
- `ALL(Product Data)` -> Returns the entire table.
- `ALL(Product Data[Product_name])` -> Returns the `product_name` column.

```ALL(FILTER(Sales, Sales[Region] = "West"))```  
When wrapped in `ALL()`, it will ignore the filter and return all rows from the Sales table, regardless of the region.

## ALL & ALLEXCEPT
- **Remove all Filters**:  
```CALCULATE([Sales], ALL(Customer))```  Removes filters from all columns.
  
- **Remove Specific filter**:  
```CALCULATE([Sales], ALL(Customer[Country]))``` Filter Sales by the column present in the Visual but not by Country Column

- **Remove All filters Except**:  
```CALCULATE([Sales], ALLEXCEPT(Customer, Customer[Country]))```  Filter Sales by Country column and ignore filter by any other column

---

## CALENDAR
```CALENDAR(Start_Date, End_Date)```  
```CALENDARAUTO()``` is used to automatically generate a date table based on the date range found in your data model.

Examples:
- `= CALENDAR("2008-05-05", "2009-05-05")`
- `= CALENDAR(MINX(Sales, [Date]), MAXX(Forecast, [Date]))`

---

## SUM and SUMX
- **SUM(Column)**: Used to sum up all values in a single column.
- **SUMX()**: An iterator function, meaning it evaluates an expression for each row of a table and sums the results.

---

## Counting Functions: COUNTA, COUNT, COUNTX, COUNTAX, DISTINCTCOUNT, COUNTBLANK, COUNTROWS
- **COUNT(ColumnName)**: Counts numeric columns, ignores blanks, works with dates.
  
- **COUNTA(ColumnName)**: Works similarly to `COUNT()` but counts string columns, ignores blanks.
  
- **COUNTX(Table, Expression)**: Evaluates expressions row by row, mostly works with numeric columns.  
  Example:  
  ```= COUNTX(FILTER(ProductData, ProductData[StockQty] > 100), ProductData[StockQty])```
  
- **DISTINCTCOUNT(ColumnName)**: Counts distinct values, including blanks as one value.
  
- **COUNTBLANK(ColumnName)**: Counts the number of blanks in a column.
  
- **COUNTROWS(table)**: Counts rows in a table, can be used with filters.

---

## FILTER
```FILTER (table, Expression)```  
Use nested filters for multiple expressions or OR/&& operators.  
Example:
```FILTER(ProductData, ProductData[Price SEK] > 10000 && ProductData[ProductCategory] = "Mountain Bikes")```

---

## EARLIER Function
Used when referencing a previous row context inside nested calculations like `SUMMARIZE` or `FILTER`.  
Example:
```VAR abc = tournaments[key_id] - 1```

RETURN
CALCULATE(MAX(tournaments[winner]),
FILTER(tournaments, tournaments[key_id] = abc))```

---

## ADDCOLUMNS
```ADDCOLUMNS(Table, Name1, Expression1, ...)```  
Returns a table with new columns created by the DAX expressions.

---

## ALLSELECTED
The `ALLSELECTED()` function returns all the values in a column or table while preserving slicers and filters but not considering filters applied within the current context.  
[More details in video](https://curbal.com/blog/glossary/allselected-dax).

---

## NULL, BLANK, ZEROS
In Power Query:
- Empty cells in text columns remain empty, numeric empty cells convert to `null`.

In table view:
- Text `null` values display as empty, numeric `null` values also appear empty.  
[More details in video](https://curbal.com/blog/glossary/blank-dax-function).

---

## CALCULATETABLE
```CALCULATETABLE(Table, filter1, filter2, …)```  
This is equivalent to:  
```FILTER(table, Filter1 && Filter2)```  
You can use it as input for iterator functions.  
Example:  
```SUMX(CALCULATETABLE(), Expression)```

---

## Rounding Functions
- **ROUND(NUMBER, 0)**: Rounds to nearest integer.
- **ROUNDUP(NUMBER, 0)**: Always rounds up.
- **ROUNDDOWN(NUMBER, 0)**: Always rounds down.
- **MROUND([Price], 0.05)**: Rounds to nearest multiple.
- **FLOOR([Number], 0.05)**: Similar to MROUND, rounds down.
- **CEILING([Number], 0.05)**: Similar to MROUND, rounds up.
- **INT([Number])**: Rounds down to integer.
- **TRUNC([Number])**: Removes decimals, no rounding.
- **ODD([Number])**: Rounds to nearest odd number.
- **EVEN([Number])**: Rounds to nearest even number.
  
For time rounding:
- **MROUND([TIME], "1:00:00")**
- **FLOOR([TIME], "00:15:00")**
- **CEILING([TIME], "00:15:00")**

---

## COMBINEVALUES
```COMBINEVALUES(<delimiter>, <expression>, <expression> [, <expression>]...)```  
Example:
```Table1[CalcColumn] = COMBINEVALUES(",", Table1[Column1], Table1[Column2])```

---

## || , IN, CONTAINSROW
- **Using ||**:  
```CALCULATE([Total Sales], DimDate[YEAR] = 1996 || DimDate[YEAR] = 1997)```

- **Using IN**:  
```CALCULATE([Total Sales], DimDate[YEAR] IN {"1996", "1997"})```

- **Using CONTAINSROW**:  
```CALCULATE([Total Sales], CONTAINSROW({"1997", "1996"}, DimDate[YEAR]))```

## CONTAINSTRING, CONTAINSTRINGEXACT

**Syntax:**

```CONTAINSTRING (Text, "String")```  
```CONTAINSTRINGEXACT (Text, "String")```

- `CONTAINSTRING`: Case-insensitive.
- `CONTAINSTRINGEXACT`: Case-sensitive.

**Examples:**

- `CONTAINSTRING(text, "Horse")` -> TRUE
- `CONTAINSTRINGEXACT(text, "horse")` -> FALSE

---

## CONVERT

**Syntax:**

```CONVERT(<Expression>, <Datatype>)```

- **Datatypes**: INTEGER, DOUBLE, STRING, BOOLEAN, CURRENCY, DATETIME.

---

## CROSSFILTER

**Syntax:**

```CROSSFILTER(<columnName1>, <columnName2>, <direction>)```

[More details](https://curbal.com/blog/glossary/crossfilter-dax)

---

## CROSSJOIN

**Syntax:**

```CROSSJOIN(<table>, <table>[, <table>]…)```

**Example:**

```DAX
CROSSJOIN(
    SELECTCOLUMNS(Products, "ProductName", Products[ProductName]),
    SELECTCOLUMNS(Regions, "RegionName", Regions[RegionName])
)


