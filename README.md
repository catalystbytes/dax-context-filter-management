# dax-context-filter-management
DAX Context and Filter Management

## DAX Context and Filter Management
### Store Sales Filter Context (DAX with Context and Filter Management)
Understanding how context and filters work is essential to writing powerful DAX formulas. This section focuses on DAX functions that help you control, override, or preserve filter contexts during evaluations. You'll learn how to use functions like ALL, REMOVEFILTERS, KEEPFILTERS, ALLSELECTED, and more to gain precise control over what data is included or excluded in your calculations. Perfect for refining reports, debugging measures, and mastering advanced DAX behavior.

Useful for Power BI reports focused on timeline analysis, operational tracking, or performance reviews based on store openings.

https://www.tiktok.com/@catalystbytes/video/7500857806594002183

```dax
Store Sales Filter Context = 

//---------------------------------------------
// 1. SECTION: ALL – Ignores All Filters on sales_fact
//---------------------------------------------
VAR TotalSales_All = CALCULATE(
    SUM(sales_fact[total_amount]),
    ALL(sales_fact) // Returns total sales ignoring all filters
)

//---------------------------------------------
// 2. SECTION: ALLCROSSFILTERED – Ignores Filters from Related Tables
//---------------------------------------------
VAR TotalSales_AllCrossFiltered = CALCULATE(
    SUM(sales_fact[total_amount]),
    ALLCROSSFILTERED(sales_fact) // Removes filters caused by relationships (e.g., product_dim or customer_dim)
)

//---------------------------------------------
// 3. SECTION: ALLEXCEPT – Keeps Only Selected Filters (Product Category)
//---------------------------------------------
VAR TotalSales_AllExcept_Category = CALCULATE(
    SUM(sales_fact[total_amount]),
    ALLEXCEPT(sales_fact, product_dim[category]) // Retains filter on category, ignores others
)

//---------------------------------------------
// 4. SECTION: ALLNOBLANKROW – Ignores Filters, Removes Blank Row
//---------------------------------------------
VAR TotalSales_NoBlankRow = CALCULATE(
    SUM(sales_fact[total_amount]),
    ALLNOBLANKROW(sales_fact) // Removes "blank" row from inactive relationships
)

//---------------------------------------------
// 5. SECTION: ALLSELECTED – Honors Explicit User Selections (e.g., slicers)
//---------------------------------------------
VAR TotalSales_AllSelected = CALCULATE(
    SUM(sales_fact[total_amount]),
    ALLSELECTED(sales_fact) // Respects slicer selections but removes visual-level filters
)

//---------------------------------------------
// 6. SECTION: REMOVEFILTERS – Removes Specific Filter (Store Region)
//---------------------------------------------
VAR TotalSales_RemoveFilters_Region = CALCULATE(
    SUM(sales_fact[total_amount]),
    REMOVEFILTERS(store_dim[region]) // Ignores region filters from store_dim
)

//---------------------------------------------
// 7. SECTION: KEEPFILTERS – Adds Filter on Brand Without Overriding Existing Ones
//---------------------------------------------
VAR TotalSales_KeepFilters_Brand = 
    VAR Result = CALCULATE(
        SUM(sales_fact[total_amount]),
        KEEPFILTERS(product_dim[brand] = "Microsoft") // Enforces filter for brand = "Microsoft" while keeping external filters
    )
    RETURN IF(ISBLANK(Result), 0, Result) // Shows 0 if no match found to avoid blank

//---------------------------------------------
// 8. OUTPUT: Combined Result on New Lines for Readability with Dynamic Formatting
//---------------------------------------------
RETURN
    "ALL = " & 
    FORMAT(TotalSales_All, "#,0") & UNICHAR(10) & 
    "ALLCROSSFILTERED = " & 
    FORMAT(TotalSales_AllCrossFiltered, "#,0") & UNICHAR(10) & 
    "ALLEXCEPT = " & 
    FORMAT(TotalSales_AllExcept_Category, "#,0") & UNICHAR(10) & 
    "ALLNOBLANKROW = " & 
    FORMAT(TotalSales_NoBlankRow, "#,0") & UNICHAR(10) & 
    "ALLSELECTED = " & 
    FORMAT(TotalSales_AllSelected, "#,0") & UNICHAR(10) & 
    "REMOVEFILTERS = " & 
    FORMAT(TotalSales_RemoveFilters_Region, "#,0") & UNICHAR(10) & 
    "KEEPFILTERS = " & 
    FORMAT(TotalSales_KeepFilters_Brand, "#,0")

```
