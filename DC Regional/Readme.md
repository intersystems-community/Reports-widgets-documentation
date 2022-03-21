<details>
<summary>Measures and dimensions</summary>

## Member Cube

### Dimensions
MonthYear - (String) user registration date in the Mon-YYYY format.  
MonthYearNum - (Int) user registration date in the MMMYYYY format.
FullDate - (Date) user registration date.
MonthYearContribution - 
MonthYearNumContribution - 
AuthorName - (String) user name.
ISCMember - (Bool) whether the user is an employee of the InterSystems.

### Measures
m_ID_distinct - number of members. 
m_Views_avg - Average views among Contributed members. 
m_Comments_sum - number of comments.
m_Member_distinct - Number of Contributed Members. 
m_CommentsAmount_sum - 
m_CommentVotes_sum -  number of likes on all comments of the posts. 

### Calculated Measures
ISCMemberStr - string representation of dimension ISCMember (InterSystems, Customers).
Formula:
CASE WHEN ISCMember = 1 THEN 'InterSystems' ELSE 'Customers' 

MemberMonthTotal - the sum of all members for the entire period up to a certain month. 
Formula: 
Sum([DateDimension].[DateDimension].CurrentMember.FirstChild : [DateDimension].[DateDimension].CurrentMember,[Measures].[m_ID_distinct])

CustomersTotalMonthly - the sum of Customers Members for the entire period up to a certain month. 
Formula:
Sum([DateDimension].[DateDimension].CurrentMember.FirstChild : [DateDimension].[DateDimension].CurrentMember,[Measures].[m_MemberCustomers_sum])

InterSystemsTotalMonthly - the sum of InterSystems members for the entire period up to a certain month. 
Formula:
Sum([DateDimension].[DateDimension].CurrentMember.FirstChild : [DateDimension].[DateDimension].CurrentMember,[Measures].[m_MembersInterSystems_sum])





## Posts cube

### Dimensions
PostType - (String) type of post (Question, Announcement, Article, and  Discussion).
MonthYearPosts - (String) the date the post was created in the Mon-YYYY format.
MonthYearNumPosts - (Int) the date the post was created in the MMMYYYY format. 
FullDatePosts - (Date) the date the post was created.
MonthYearView - (String) date the post was viewed in the Mon-YYYY format.  
MonthYearViewNum - (Int) date the post was viewed in the MMMYYYY format.
NamePost - (String) the post title.
LinkPost - (String) the link to the post.
WeekYear - (Int) number of the week in the year in which the post was created. 

### Measures
m_AcceptedAnswerAmount_sum - 
m_CountArticle_sum - 
m_AvgVote_sum - 
m_CacheTag_sum - the number of views of a post with the tag “Caché”. 
m_CommentsAmont_sum - 
m_CommentsContribution_sum - 
m_CountDiscussion_sum - the number of posts of type “Discussion”.
m_EnsembleTag_sum - the number of posts with the “Ensemble” tag. 
m_HealthShareTag_sum - the number of views of posts with the “HealthShare” tag.
m_InterSystemsIRISTag_sum - the number of views of posts with the “InterSystems IRIS” tag. 
m_ID_distinct - the number of posts.
m_CountQuestions_sum - the number of posts of type “Questions”.
m_CountTagCache_sum - the number of views of posts with the “Caché” tag. 
m_CountTagEnsemble_sum - the number of views of posts with the “Ensemble” tag.
m_CountTagHealthShare_sum - the number of views of posts with the “HealthShare” tag.
m_CountTagIRIS_sum - the number of views of posts with the “InterSystems IRIS” tag.



### Calculated Measures
TotalArticlesMonth - the sum of InterSystems members for the entire period up to a certain month.
Formula: 
Sum([DateDimensionPosts].[DateDimensionPosts].CurrentMember.FirstChild : [DateDimensionPosts].[DateDimensionPosts].CurrentMember,[Measures].[m_CountArticle_sum])
TotalPostsMonth - 
Formula:
Sum([DateDimensionPosts].[DateDimensionPosts].CurrentMember.FirstChild : [DateDimensionPosts].[DateDimensionPosts].CurrentMember,[Measures].[m_ID_distinct])

TotalQuestionsMonth - 
Formula:
Sum([DateDimensionPosts].[DateDimensionPosts].CurrentMember.FirstChild : [DateDimensionPosts].[DateDimensionPosts].CurrentMember,[Measures].[m_CountQuestions_sum])

TotalViewsMonth - 
Formula
Sum([MonthYearView].[MonthYearView].CurrentMember.FirstChild : [MonthYearView].[MonthYearView].CurrentMember,[Measures].[m_Delta_sum])

Formula field
CurrentDate - full current date.
Today()

CurrentMonthNum - current month date in MMMYYYY format.  
Date a = FirstDayOfMonth(@CurrentDate) 
return (Year(a) * 100) + Month(a)

CurrentMonthYearNum - last month date in MMMYYYY format.
Date a = FirstDayOfMonth(@CurrentDate) - 1;
return (Year(a) * 100) + Month(a)

Last6Month - a date that was 6 months ago in the MMMYYYY format.
Date a = FirstDayOfMonth(Today()-Day()-210);
return (Year(a) * 100) + Month(a)

 </details>

## General information

Any registered user who has been active in the community is counted as a member of the community. A user can be a member of several communities at the same time.

Removed data about unpublished and deleted posts, as well as Digest type posts from data about posts and their views.

## Page 2

### Members Total table
_The table shows the dynamics of the number of members, new members per month, new contributed members per month._ 

<details>
<summary>For developers</summary>

Table with MemberMonthTotal, m_ID_distinct and  m_Member_distinct in Display, MonthYear in Group. Only the last 6 months are taken by the Select Bottom N method, and the current month is excluded by the filter MonthYearNum != @CurrentMonthYear (MMMYYYY)  

</details>

### Members Total bar chart
_Members Total shows the number of users registered on the site at the end of each month._ 

<details>
<summary>For developers</summary>

Bar chart with MemberMonthTotal in Show Values, MonthYear in Category. Only the last 6 months are taken into MonthYear using the Select Bottom N function, and current month skipped by filter.

</details>

### Post Total table
_The table shows the cumulative total and the number of new posts per month by Post type (Questions, Article, Announcement, Discussion)_ 

<details>
<summary>For developers</summary>

Table with TotalPostsMonth, TotalQuestionsMonth, TotalArticlesMonth, posts_m_ID_distinct, m_CountQuestions_sum, m_CountArticle_sum, m_CountAnnouncement_sum and m_CountDiscussion_sum in Display, MonthYearPosts in Group. Only the last 6 months are taken by the Select Bottom N method, and the current month is excluded by the filter MonthYearNum != @CurrentMonthYear (MMMYYYY)

</details>

### Post Total bar chart
_This chart shows the number of new posts by type for each month._ 

<details>
<summary>For developers</summary>

Bar chart with New Questions, New Articles, New Announcements and New Discussions in Show Values, MonthYearPosts in Category. Only the last 6 months are taken into MonthYearPosts using the Select Bottom N function, and the current month skipped by filter.

</details>

## Page 3

### View Total table
_The table shows the total number of post views, new views per month, the average number of Article views, and the average number of Questions views. Showing the last 6 months._  

<details>
<summary>For developers</summary>

4 tables with MonthYearView in Group.Only the last 6 months are taken with the exception of the current one. The following filter is used for this: 
MonthYearViewNum > @Last6Month
AND
MonthYearViewNum != @CurrentMonthNum
For the first table: TotalViewsMonth and m_Delta_sum in Display.
For the second table:  AvgArticleViews in Display. This is the Crosstab Formula field, formula : @(@PostType:"ARTICLE", Sum(@posts_m_Views_avg)).
For the third table AvgQuestionsViews in Display. This is the Crosstab Formula field, formula : @(@PostType:"QUESTION", Sum(@posts_m_Views_avg)).

</details>

### View Total bar chart
_The chart shows the number of new views of all community posts per month._ 

<details>
<summary>For developers</summary>

Bar chart with m_Delta_sum in Show Values and MonthYearView in Category. Only the last 6 months are taken into MonthYearView using the Select Bottom N function, and the current month skipped by filter.

</details>

## Page 4

### Members Base bar chart

_This diagram shows the breakdown of all new members per month into 2 categories - InterSystems employees and customers._ 

### Contributors Monthly bar chart

_This chart shows the number of active members per month._


## Page 5

### Customers Contributors Monthly bar chart

_This chart shows the number of active customers per month._

### InterSystems Contributors Monthly bar chart

_This chart shows the number of active InterSystems employees per month._

### Posting Contribution

_This chart shows the number of new posts and new comments on all posts per month._



## Page 6

### Postings by Type

_This chart shows a breakdown of the number of new posts per month by post type._ 

### Products Contribution Breakdown: Caché, Ensemble, HealthShare, IRIS

_This chart shows a breakdown of the number of new posts per month by project._

### Articles by InterSystems And Customers breakdown

_This chart shows a breakdown of the number of new Articles per month by author status: InterSystems employee or customer._ 


## Page 7

### Questions ny InterSystems and Customers Breakdown

_This chart shows a breakdown of the number of new Questions per month by author status: InterSystems employee or customer._  

### Customers Weekly Contribution Level in Questions

_This chart shows the number of new Questions per week._

### Comments and Answers

_This chart shows a breakdown of the number of new Comments on all posts per month by author status: InterSystems employee or customer._  

## Page 8

### Contribution

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day._

### InterSystems

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day. Only data on the activity of InterSystems employees was taken._

### Customers

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day. Only data on the activity of customers was taken._

## Page 9

### Best Contributors: Customers

_The table shows the most active Customers for the month._

Points are the sum of the following:
    The number of posts written by the member.
    The number of comments written by the member.
    The number of views of posts written by a member.
    The number of votes for comments and posts.
    The number of given correct answers to questions.
    The number of comments on posts written by the member.
    The number of times a member's posts have been added to favorites.


### Best Contributors: InterSystems

_The table shows the most active InterSystems employees for the month._

Points are the sum of the following:
    The number of posts written by the member.
    The number of comments written by the member.
    The number of views of posts written by a member.
    The number of votes for comments and posts.
    The number of given correct answers to questions.
    The number of comments on posts written by the member.
    The number of times a member's posts have been added to favorites.

## Page 10

### Best Contributors

_This chart shows the distribution of points of the most active members of the community by category._

### Best Contributors Ever

_This chart shows the most active community members of all time._

## Page 11

### Content Consumption Views by Content Type

_This chart shows the number of views per month of all posts and questions._

### Unique Views Per Every Post

_This chart shows the average number of views among community posts._

## Page 12

### Total Views Monthly

_This chart shows the number of views of all community posts in a month._

### Total Views Monthly: Caché, Ensemble, HealthShare, IRIS

_This chart shows the distribution of views across the projects to which the posts belong._

## Page 13
