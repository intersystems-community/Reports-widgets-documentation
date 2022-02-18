## Used in report
### m_count_sum
The Member has a Language field that lists all the languages he speaks. For each of the languages there is a column in which 1 is set if this language is present in the user's languages. For each column, the number of these units is summed up in measure, for English it is m_countEN_sum, for other languages it is similar. We will refer to these measures by the common name m_count_sum.  

When we group this field by time period, we get the number of new users for that period   

### MonthYear
Dates grouped by month are in MMM-yyyy format and are stored in a separate MonthYear dimension
### MonthYearNum
Dates grouped by month are in yyyymm format and are stored in a separate MonthYearNum dimension
### MonthYear custom sort
To display months in MonthYear  in an appropriate way, we use custom sort by MonthYearNum in descending order in all charts and tables.
### CurrDate
CurrDate is a Logi Function “return Today()”
### FullDate
FullDate is dimension where is the user registration date stored
### m_post_id_count
Measure from AtScale, which count all posts id

### group_post_sum
group_post_sum is the Summary on m_post_id_count Group by posts_lang
### posts_lang
posts_lang is the Formula to convert database post langs names in displayed in report
### Post_date
Post_date is the formula “FirstDayOfMonth(@posts_FullDate)”, new idea 

## List 2
### New Members bar chart
_A member counts in every community in which (s)he participates. All members participate in the English community. New members are counted as users who registered this month and can be counted in multiple communities at once._

The chart is built as m_count_sum in Show Values, divided into categories (Category) by MonthYear. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### New Members table
_Numeric representation of the chart_

Built in a similar way: measures m_count_sum in Display, MonthYear in Group.

## List3
### Total Members
_Total Members shows the number of all community members by the end of the month_

Total Members is Stacked bar chart, displaying the running total of m_count_sum divided into categories (Category) by MonthYear. Logi does not have the ability to use the running total function, so we will implement it ourselves.

For each m_count_sum, we create a set of parameters that store the value of all members at the beginning of each month. This is implemented through an SQL query like:

SELECT DISTINCT members.m_CountEN_sum FROM members Where DateDiff('m', members.FullDate, @CurrDate) >=1

For each parameter set there is a function like  

    if (MonthYear == month_1):  
        Param_1  
    else if (MonthYear == month_2):  
        Param_2  

Thanks to this function, when the chart displays the next group by MonthYear, the value of the desired parameter is displayed.

So functions are in Show Values, MonthYear is in Category. Only the last 12 months are taken into MonthYear using the Select Bottom N function.

### Total Members All Time
Total Members All Time shows the number of all members of the community at the current moment

Pie chart where functions are in Show Values, MonthYear is in Category, but from MonthYear the bottom 1 is selected to show the sum of members for the last month.

The table next to the diagram is built similarly: functions are in Display, MonthYear is in Group and from MonthYear the bottom 1 is selected to show the sum of members for the last month.

KPI for the chart shows the number of English users, as all users belong to this group.

## List 4
### Total Members for last year
_Total Members for last year show the number of community members registered this year_

Here we use other functions in which the parameter for the eleventh month (Param_11) is subtracted from the parameter for the current month (Param_0).

Pie chart where functions are in Show Values, MonthYear is in Category, but from MonthYear the bottom 1 is selected in order to have a group that is needed to combine several functions into one diagram

### Table
_The table shows how many members there are in each community at the  current moment, how many there  were last year, how fast much the community grew in a year, percentage of growth per year, how many members were at the end of the previous month, how fast much the community grew in the current month, growth percentage for month._

Each table row is a separate table. The three tables for current and previous month and previous year use the formulas for Total Members All Time in Display, MonthYear is in Group and from MonthYear the bottom 1 is selected to get the value for the desired month.

Growth tables use a formulas Param_0 – Param_11 for the year and Param_0 – Param_1 for month.

Growth % tables use a formula (Param_0 – Param_11)/ Param_0 for the year and (Param_0 – Param_1)/ Param_0 for month.

In this tables MonthYear is in Group to have a group that is needed to avoid this bug  
(https://github.com/teccod/Logi-Atscale-Tableau-Issues/issues/21)

## List 5
### New Posts bar chart
_New Posts shows how many new posts were published per month_

Bar chart with posts_lang in Show Values, MonthYear in Category. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### Table
_Numeric representation of the chart_

Crosstab with m_post_id_count in summarys, Post_date in Rows and posts_lang in Columns. Fulter: to display only last 6 months.

## List 6
### Total Posts
_Running ? Total Posts of any type?? shows how many posts were published for the entire period until the end of the specified month_

Total Posts is a Stacked bar chart made in the same way as Total Members, as Logi does not have the ability to use the running total function.

We create a set of parameters for each Lang that store the value of all posts at the beginning of each month. This is implemented through an SQL query like:

SELECT DISTINCT posts.m_post_id_count FROM posts WHERE DateDiff('m', posts.FullDate, @CurrDate)>0 AND posts.Lang = 'CN'

For each parameter set there is a function like

    if (MonthYear == month_1):  
        Param_1  
    else if (MonthYear == month_2):  
        Param_2  

Thanks to this function, when the chart displays the next group by MonthYear, the value of the desired parameter is displayed.

These functions are combined into one function like:

    if (@posts_lang == "EN")  
        @posts_sum_en  
    else if (@posts_lang == "ES")  
        @posts_sum_es  

So functions are in Show Values, MonthYear is in Category. Only the last 12 months are taken into MonthYear using the Select Bottom N function.

In Y axes format minimum and maximum values edited to improve the visual of the chart.

### Total Posts All Time
_Total Posts shows how many posts were published for the entire period until the end of the current month_

Pie chart with posts_lang in Show Values, posts_lang in Category.

### Table
_Numeric representation of the chart_

Table with posts_lang in Display, m_post_id_count in Group.

### Total Posts for last year
_Total Posts shows how many posts were published in the last 12 months_

Pie chart with posts_lang in Show Values, posts_lang in Category. Filter: only 12 last months

### Table
_The table shows how many posts were published in each community at the moment, how many were last year, how many posts were published per year, the percentage of growth per year, how many posts were at the end of the previous month, how many posts were published this month, the percentage of growth per month._

Each table row is a separate table. All rows, which are not growth percentage, is crosstabs, growth percentage rows is tables.

All crosstabs have m_id_distinct in summary, posts_lang in columns.

The difference in the displayed values is formed using filters. For current month – no filters, for last year – filter Date< last year, for last month – filter Date<last month, for yearly growth – filter Date >= last year, for monthly growth – dilter Date >= last month.

Growth percentage tables have formulas (Param_0 – Param_11)/ Param_0 for the year and (Param_0 – Param_1)/ Param_0 for month for each lang in display, fill date with special group (1000 years) in group to avoid bug.

## Lists 7-8
m_Count_sum – group of metrics, summing up all publications of their type.

### Articles line chart
_Articles shows how many new articles were published per month_

Articles – is Summary which sum m_CountArticles_sum group by MonthYear.

Bar chart with Articles in Show Values, posts_lang in Clustering. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### Table
_Numeric representation of the chart_

Crosstab with m_CountArticles_sum in summarys, Post_date in Rows and posts_lang in Columns. Fulter: to display only last 6 months.

### Questions line chart
_Questions shows how many new questions were published per month_

Questions – is Summary which sum m_CountQuestions _sum group by MonthYear.

Bar chart with Questions in Show Values, posts_lang in Clustering. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### Table
_Numeric representation of the chart_

Crosstab with m_CountQuestions _sum in summarys, Post_date in Rows and posts_lang in Columns. Fulter: to display only last 6 months.

### Announcements line chart
_Announcements shows how many new Announcements were published per month_

Announcements – is Summary which sum m_CountAnnouncement_sum group by MonthYear.

Bar chart with Announcements in Show Values, posts_lang in Clustering. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### Table
_Numeric representation of the chart_

Crosstab with m_CountAnnouncement_sum in summarys, Post_date in Rows and posts_lang in Columns. Fulter: to display only last 6 months.

### Discussions line chart
_Discussions shows how many new discussions were published per month_

Discussions – is Summary which sum m_CountDiscussion_sum group by MonthYear.

Bar chart with Discussions in Show Values, posts_lang in Clustering. Only the last 6 months are taken into MonthYear using the Select Bottom N function.

### Table
_Numeric representation of the chart_

Crosstab with m_CountDiscussions_sum in summarys, Post_date in Rows and posts_lang in Columns. Fulter: to display only last 6 months.

## List 9
Views have another datasours – IRIS directly.

**New_Views_monthly** – summary which sum Delta group by Date, special function – for each month. This is the equivalent of m_count_sum measures in the first datasours.  
**posts_lang** - formula to convert database post langs names displayed in report.  
**Total_Views** - summary which sum Delta group by Lang  

### New Views
_New Views shows how many views were for the specified month for all publications_

Bar chart with New_Views_monthly in Show Values and Lang in Clustering. Lang formed by posts_lang - formula to convert database post langs names in displayed in report. Only the last 6 months are taken into Date using the Select Bottom N function.

## List 10
### Total Views
_Total Views shows how many views all posts had in total_

Pie chart with Total_Views in Show Values.

### Total Views for last year
_Total Views for last year shows how many total views all posts had in the last 12 months_

Pie chart with Total_Views in Show Values. Filter: Date >= last year

### Table
_The table shows how many views all posts had in total and how many views all posts had in the last 12 months._

2 crosstabs with sum of Delta in summary, posts_lang in columns. For current month – no filter, for YTY Growth: Date >= last year
