## Used in report
### m_count_sum
The Member has a Language field that lists all the languages he speaks. For each of the languages there is a column in which 1 is set if this language is present in the user's languages. For each column, the number of these units is summed up in measure, for English it is m_countEN_sum, for other languages it is similar. We will refer to these measures by the common name m_count_sum.  

When we group this field by time period, we get the number of new users for that period   

#### MonthYear
Dates grouped by month are in MMM-yyyy format and are stored in a separate MonthYear dimension
#### MonthYearNum
Dates grouped by month are in yyyymm format and are stored in a separate MonthYearNum dimension
#### MonthYear custom sort
To display months in [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear)  in an appropriate way, we use custom sort by MonthYearNum in descending order in all charts and tables.
#### CurrDate
CurrDate is a Logi Function “return Today()”
#### FullDate
FullDate is dimension where is the user registration date stored
#### m_post_id_count
Measure from AtScale, which count all posts id

#### group_post_sum
group_post_sum is the Summary on [m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) Group by [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang)
#### posts_lang
posts_lang is the Formula to convert database post langs names in displayed in report
#### Post_date
Post_date is the formula “FirstDayOfMonth(@posts_[FullDate](https://github.com/teccod/Reports-widgets-documentation#fulldate))”, another way to get the date of the MMM-yyyy format

## Page 2
### New Members bar chart
_A member counts in every community in which (s)he participates. All members participate in the English community. New members are counted as users who registered this month and can be counted in multiple communities at once._

<details>
<summary>For developers</summary>
 
The chart is built as [m_count_sum](https://github.com/teccod/Reports-widgets-documentation#m_count_sum) in Show Values, divided into categories (Category) by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear). Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.
 
</details>

### New Members table

Built in a similar way: measures [m_count_sum](https://github.com/teccod/Reports-widgets-documentation#m_count_sum) in Display, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) in Group.

## Page 3
### Total Members
_Total Members shows the number of all community members by the end of the month_

<details>
<summary>For developers</summary>
 
Total Members is Stacked bar chart, displaying the running total of [m_count_sum](https://github.com/teccod/Reports-widgets-documentation#m_count_sum) divided into categories (Category) by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear). Logi does not have the ability to use the running total function, so we will implement it ourselves.

For each [m_count_sum](https://github.com/teccod/Reports-widgets-documentation#m_count_sum), we create a set of parameters that store the value of all members at the beginning of each month. This is implemented through an SQL query like:

SELECT DISTINCT members.m_CountEN_sum FROM members Where DateDiff('m', members.[FullDate](https://github.com/teccod/Reports-widgets-documentation#fulldate, @[CurrDate](https://github.com/teccod/Reports-widgets-documentation#currdate)) >=1

For each parameter set there is a function like  

    if ([MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) == month_1):  
        Param_1  
    else if ([MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) == month_2):  
        Param_2  

Thanks to this function, when the chart displays the next group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear), the value of the desired parameter is displayed.

So functions are in Show Values, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Category. Only the last 12 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.
 
</details>


### Total Members All Time
_Total Members All Time shows the number of all members of the community at the current moment_

<details>
<summary>For developers</summary>

Pie chart where functions are in Show Values, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Category, but from [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) the bottom 1 is selected to show the sum of members for the last month.

The table next to the diagram is built similarly: functions are in Display, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Group and from [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) the bottom 1 is selected to show the sum of members for the last month.

KPI for the chart shows the number of English users, as all users belong to this group.

</details>

## Page 4
### Total Members for last year
_Total Members for last year show the number of community members registered in previous 12 months_

<details>
<summary>For developers</summary>

Here we use other functions in which the parameter for the eleventh month (Param_11) is subtracted from the parameter for the current month (Param_0).

Pie chart where functions are in Show Values, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Category, but from [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) the bottom 1 is selected in order to have a group that is needed to combine several functions into one diagram


</details>

### Table

<details>
<summary>For developers</summary>

Each table row is a separate table. The three tables for current and previous month and previous year use the formulas for Total Members All Time in Display, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Group and from [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) the bottom 1 is selected to get the value for the desired month.

Growth tables use a formulas Param_0 – Param_11 for the year and Param_0 – Param_1 for month.

Growth % tables use a formula (Param_0 – Param_11)/ Param_0 for the year and (Param_0 – Param_1)/ Param_0 for month.

In this tables MonthYear is in Group to have a group that is needed to avoid this bug  
(https://github.com/teccod/Logi-Atscale-Tableau-Issues/issues/21)

</details>

## Page 5
### New Posts bar chart
_New Posts shows how many new posts were published per month_

<details>
<summary>For developers</summary>

Bar chart with [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Show Values, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) in Category. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

### Table

<details>
<summary>For developers</summary>
Crosstab with [m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) in summarys, [Post_date](https://github.com/teccod/Reports-widgets-documentation#post_date) in Rows and [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Columns. Fulter: to display only last 6 months and not display current month.
</details>

## Page 6
### Total Posts
_Running Total Posts of any type shows how many posts were published for the entire period until the end of the specified month_

<details>
<summary>For developers</summary>
Total Posts is a Stacked bar chart made in the same way as Total Members, as Logi does not have the ability to use the running total function.

We create a set of parameters for each Lang that store the value of all posts at the beginning of each month. This is implemented through an SQL query like:

SELECT DISTINCT posts.[m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) FROM posts WHERE DateDiff('m', posts.[FullDate](https://github.com/teccod/Reports-widgets-documentation#fulldate, @[CurrDate](https://github.com/teccod/Reports-widgets-documentation#currdate)>0 AND posts.Lang = 'CN'

For each parameter set there is a function like

    if ([MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) == month_1):  
        Param_1  
    else if ([MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) == month_2):  
        Param_2  

Thanks to this function, when the chart displays the next group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear), the value of the desired parameter is displayed.

These functions are combined into one function like:

    if (@[posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) == "EN")  
        @posts_sum_en  
    else if (@[posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) == "ES")  
        @posts_sum_es  

So functions are in Show Values, [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) is in Category. Only the last 12 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

In Y axes format minimum and maximum values edited to improve the visual of the chart.
</details>

### Total Posts All Time
_Total Posts shows how many posts were published for the entire period until the end of the current month_

<details>
<summary>For developers</summary>

Pie chart with [m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Category.

</details>

### Table

<details>
<summary>For developers</summary>

Table with [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Display, [m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) in Group. Fulter: not to display current month.

</details>

## Page 7
### Total Posts for last year
_Total Posts shows how many posts were published in the last 12 months_

<details>
<summary>For developers</summary>

Pie chart with [m_post_id_count](https://github.com/teccod/Reports-widgets-documentation#m_post_id_count) in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Category. Filters: only 12 last months and not to display current month.

</details>

### Table

<details>
<summary>For developers</summary>

Each table row is a separate table. All rows, which are not growth percentage, is crosstabs, growth percentage rows is tables.

All crosstabs have m_id_distinct in summary, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in columns.

The difference in the displayed values is formed using filters. For current month – filter Date < current month, for last year – filter Date < last year, for last month – filter Date < last month, for yearly growth – filter Date >= last year, for monthly growth – dilter Date >= last month.

Growth percentage tables have formulas (Param_0 – Param_11)/ Param_0 for the year and (Param_0 – Param_1)/ Param_0 for month for each lang in display, fill date with special group (1000 years) in group to avoid bug.

</details>

## Pages 8
m_Count_sum – group of metrics, summing up all publications of their type.

### Articles line chart
_Articles shows how many new articles were published per month_

<details>
<summary>For developers</summary>


Articles – is Summary which sum m_CountArticles_sum group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear).

Line chart with Articles in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Clustering. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

### Table

<details>
<summary>For developers</summary>

Crosstab with m_CountArticles_sum in summarys, [Post_date](https://github.com/teccod/Reports-widgets-documentation#post_date) in Rows and [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Columns. Fulter: to display only last 6 months and not display current month.

</details>

### Questions line chart
_Questions shows how many new questions were published per month_

<details>
<summary>For developers</summary>

Questions – is Summary which sum m_CountQuestions_sum group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear).

Line chart with Questions in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Clustering. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

### Table

<details>
<summary>For developers</summary>

Crosstab with m_CountQuestions_sum in summarys, [Post_date](https://github.com/teccod/Reports-widgets-documentation#post_date) in Rows and [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Columns. Fulter: to display only last 6 months and not display current month.

</details>

## Page 9
### Announcements line chart
_Announcements shows how many new Announcements were published per month_

<details>
<summary>For developers</summary>

Announcements – is Summary which sum m_CountAnnouncement_sum group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear).

Line chart with Announcements in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Clustering. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

### Table

<details>
<summary>For developers</summary>

Crosstab with m_CountAnnouncement_sum in summarys, Post_date in Rows and [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Columns. Fulter: to display only last 6 months and not display current month.

</details>

### Discussions line chart
_Discussions shows how many new discussions were published per month_

<details>
<summary>For developers</summary>

Discussions – is Summary which sum m_CountDiscussion_sum group by [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear).

Line chart with Discussions in Show Values, [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Clustering. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

### Table

<details>
<summary>For developers</summary>

Crosstab with m_CountDiscussions_sum in summarys, [Post_date](https://github.com/teccod/Reports-widgets-documentation#post_date) in Rows and [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang) in Columns. Fulter: to display only last 6 months and not display current month.

</details>

## Page 10
### Questions Situation pie chart
_This chart shows the distribution of the total number of questions into 3 categories: answer accepted, answer received and no answer._

<details>
<summary>For developers</summary>

Question_situation - is Summary on m_CountQuestions_sum group by QuestionType

Pie chart with Question_situation in Show Values. Fulter: not to display current month.

</details>

### Questions over time bar chart
_This chart shows the distribution of question status over the past 6 months._

<details>
<summary>For developers</summary>

Bar chart Questions summary in Show Values and QuestionType in Series. Only the last 6 months are taken into [MonthYear](https://github.com/teccod/Reports-widgets-documentation#monthyear) using the Select Bottom N function, and current month skipped by Skip First option.

</details>

## Page 11
Views have another datasours – IRIS directly. Querry have the filter not to show data for current month.

#### New_Views_monthly

This is summary which sum Delta group by Date, special function – for each month. This is the equivalent of [m_count_sum](https://github.com/teccod/Reports-widgets-documentation#m_count_sum) measures in the first datasours.  
#### posts_lang

This is formula to convert database post langs names displayed in report.  
#### Total_Views  

This is summary which sum Delta group by Lang  

#### New Views
_New Views shows how many views were for the specified month for all publications_

<details>
<summary>For developers</summary>

Bar chart with [New_Views_monthly](https://github.com/teccod/Reports-widgets-documentation#new_views_monthly) in Show Values and Lang in Clustering. Lang formed by [posts_lang](https://github.com/teccod/Reports-widgets-documentation#posts_lang-1) - formula to convert database post langs names in displayed in report. Only the last 6 months are taken into Date using the Select Bottom N function.

</details>

## Page 12
### Total Views
_Total Views shows how many views all posts had in total_

<details>
<summary>For developers</summary>

Pie chart with [Total_Views](https://github.com/teccod/Reports-widgets-documentation#total_views) in Show Values.

</details>

### Total Views for last year
_Total Views for last year shows how many total views all posts had in the last 12 months_

<details>
<summary>For developers</summary>

Pie chart with [Total_Views](https://github.com/teccod/Reports-widgets-documentation#total_views) in Show Values. Filter: Date >= last year

</details>

### Table

<details>
<summary>For developers</summary>

Post_langs_order - is a formula, which assigns each language a serial number to organize the display order 

Year_growth formula for each language calculates the percentage of growth for the period using the formula (Param1 - Param2)/Param1 * 100 where Param1 - the sum of all views for the period up to the last month and Param2 - the sum of all views for the period up to the 13 months

Month_growth formula for each language calculates the percentage of growth for the period using the formula (Param1 - Param2)/Param1 * 100 where Param1 - the sum of all views for the period up to the last month and Param2 - the sum of all views for the period up to the 2 months

The table consists of 7 crosstabs. 5 crosstabs with sum of Delta in summary, Post_langs_order in columns. For current month – no filter, for YTY Growth: Date >= last year, for MTM Growth: Date >= last month, for last year Date < last year, for last month Date < last month.

1 crosstabs have maximum of Year_growth formulas in summary, Post_langs_order in columns.
1 crosstabs have maximum of Month_growth formulas in summary, Post_langs_order in columns.

</details>
