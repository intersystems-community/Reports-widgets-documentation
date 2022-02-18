# reports-widgets-documentation
Documentation for reports and widgets
## Used in report
#### m_count_sum
The Member has a Language field that lists all the languages he speaks. For each of the languages there is a column in which 1 is set if this language is present in the user's languages. For each column, the number of these units is summed up in measure, for English it is m_countEN_sum, for other languages it is similar. We will refer to these measures by the common name m_count_sum.  

When we group this field by time period, we get the number of new users for that period   

#### MonthYear
Dates grouped by month are in MMM-yyyy format and are stored in a separate MonthYear dimension
#### MonthYearNum
Dates grouped by month are in yyyymm format and are stored in a separate MonthYearNum dimension
#### MonthYear custom sort
To display months in MonthYear  in an appropriate way, we use custom sort by MonthYearNum in descending order in all charts and tables.
#### CurrDate
CurrDate is a Logi Function “return Today()”
#### FullDate
FullDate is dimension where is the user registration date stored
#### m_post_id_count
Measure from AtScale, which count all posts id

#### group_post_sum
group_post_sum is the Summary on m_post_id_count Group by posts_lang
#### posts_lang
posts_lang is the Formula to convert database post langs names in displayed in report
#### Post_date
Post_date is the formula “FirstDayOfMonth(@posts_FullDate)”, new idea 

## List 2
#### New Members bar chart
A member counts in every community in which (s)he participates. All members participate in the English community. New members are counted as users who registered this month and can be counted in multiple communities at once.

The chart is built as m_count_sum in Show Values, divided into categories (Category) by MonthYear. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

#### New Members table
Numeric representation of the chart

Built in a similar way: measures m_count_sum in Display, MonthYear in Group.

