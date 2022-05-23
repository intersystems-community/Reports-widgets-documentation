<details>
<summary>Measures and dimensions</summary>

## Member Cube
	
### Measures
- m_ID_distinct - number of members. 
- m_Member_distinct - Number of Contributed Members. 

### Dimensions
- MonthYear - (String) user registration date in the Mon-YYYY format.  
- MonthYearNum - (Int) user registration date in the YYYYMM format.
- FullDateMember - (Date) user registration date.
- Email - (String) user email.
- Name - (String) user name.
- Link - (String) link to user profile. 
- ISCMember - (Bool) whether the user is an employee of the InterSystems.
- ISCMemberStr - (String) string type of user status InterSystems or Customers.
	

### Calculated Measures
- ISCMemberStr - String representation of dimension ISCMember (InterSystems, Customers).
	```
	CASE WHEN ISCMember = 1 THEN 'InterSystems' ELSE 'Customers' 
	```
 
- TotalMemberMonth - The sum of all members for the entire period up to a certain month.
	```
	Sum([DateDimensionMember].[DateDimensionMember].CurrentMember.FirstChild : 
		[DateDimensionMember].[DateDimensionMember].CurrentMember,[Measures].[m_ID_distinct])
	```
	
- MTM_Members_num - Monthly growth by users
	```
	[Measures].[TotalMemberMonth] - 
		([DateDimensionMember].[DateDimensionMember].CurrentMember.PrevMember,[Measures].[TotalMemberMonth])
	```
	
- MTM_Members_percent - Percentage monthly growth by users
	```
	Divide(([Measures].[TotalMemberMonth] - 
		([DateDimensionMember].[DateDimensionMember].CurrentMember.PrevMember,[Measures].[TotalMemberMonth])), 
		([DateDimensionMember].[DateDimensionMember].CurrentMember.PrevMember,[Measures].[TotalMemberMonth]), 0)
	```

## Views cube

### Dimensions
- MonthYear - (String) The date the post was viewed in the format Mon-YYYY.
- MonthYearNum - (int) The date the post was viewed in the format YYYYMM.
- YearView - (String) The year in which the post was viewed.
- NamePost - (String) Post title.
- Tags - (String) Tags used in the post.
- LinkPost - (String) Community post link
- PostType - (String) type of post (Question, Announcement, Article, and  Discussion).

### Measures
- m_CacheTag_sum - The number of views of posts with the Cache tag.
- m_EnsembleTag_sum - The number of views of posts with the Ensemble tag.
- m_HealthShareTag_sum - The number of views of posts with the Health Share tag
- m_InterSystemsIRISTag_sum - The number of views of posts with the InterSystems IRIS tag.
- m_Views_avg - Average views
- m_Delta_sum - 
- m_Views_sum - Number of views.

### Calculated Measures
- MTM_Delta_num
	```
	[Measures].[m_Delta_sum] - 
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Delta_sum])
	```
  
- MTM_Delta_percent
	```
	Divide(([Measures].[m_Delta_sum] -
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Delta_sum])), 
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Delta_sum]), 0)
	```
	
- MTM_ViewsAVG_num - Monthly growth in average view.
	```
	[Measures].[m_Views_avg] - 
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Views_avg])
	```
	
- MTM_ViewsAVG_percent - Percentage growth in average view per month.
	```
	Divide(([Measures].[m_Views_avg] - 
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Views_avg])), 
		([DateDimensionView].[DateDimensionView].CurrentMember.PrevMember,[Measures].[m_Views_avg]), 0)
	```

- TotalViewsMonth - Cumulative views per month.
	```
	Sum([DateDimensionView].[DateDimensionView].CurrentMember.FirstChild : 
		[DateDimensionView].[DateDimensionView].CurrentMember,[Measures].[m_Delta_sum])
	```

## ContributedMembers cube

### Dimensions
- YearContributors - Year in which the user became a Contributor.
- ISCMember - (Bool) whether the user is an employee of the InterSystems.
- ISCMemberStr - (String) string type of user status InterSystems or Customers.
- FullDateContribution - (Date) Date the user was added to Contributor.
- MonthYear - (String) Date the user was added to the Contributor in Mon-YYYY format.
- MonthYearNum - (int) Date the user was added to the Contributor in YYYYMM format.
- Email - (String) user email.
- Name - (String) user name.
- Link - (String) link to user profile.
	
### Measures
- m_Comments_sum - 
- m_CommentsAmount_sum - 
- m_CommentVotes_sum - 
- m_Member_distinct - Number of Contributed Members.
- m_PostFavs_sum - 
- m_Overall_sum - 
- m_Posts_sum - Number of posts created by the user
- m_PostVotes_sum - 
- m_Views_sum - Number of post views for Contributed Members.
	
	
## Posts cube

### Dimensions
- PostType - (String) type of post (Question, Announcement, Article, and  Discussion).
- MonthYear - (String) the date the post was created in the Mon-YYYY format.
- MonthYearNum - (Int) the date the post was created in the MMMYYYY format. 
- FullDate - (Date) the date the post was created.
- WeekYear - (Int) number of the week in the year in which the post was created.
- NamePost - (String) the post title.
- LinkPost - (String) the link to the post.
- Tags - (String) Tags used in the post.
- ISCMember - (Bool) whether the user is an employee of the InterSystems.
- ISCMemberStr - (String) string type of user status InterSystems or Customers.

### Measures	
- m_AcceptedAnswerAmount_sum - 
- m_CountArticle_sum - 
- m_AvgVote_sum - 
- m_Views_sum - 
- m_CountAnnouncement_sum -
- m_CommentsAmount_sum - 
- m_CommentsContribution - 
- m_ID_distinct - the number of posts.
- m_CountDiscussion_sum - the number of posts of type “Discussion”.
- m_CountQuestions_sum - the number of posts of type “Questions”.
- m_CountTagCache_sum - the number of posts with the “Caché” tag. 
- m_CountTagEnsemble_sum - the number of posts with the “Ensemble” tag.
- m_CountTagHealthShare_sum - the number posts with the “HealthShare” tag.
- m_CountTagIRIS_sum - the number of posts with the “InterSystems IRIS” tag.


### Calculated Measures
- MTM_Articles_num - Monthly growth of posts with type Articles.
	```
	[Measures].[m_CountArticle_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountArticle_sum])
	```
	
- MTM_Articles_percent - Monthly growth of posts with type Articles in percentage.
	```
	Divide( ([Measures].[m_CountArticle_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountArticle_sum])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountArticle_sum]), 0)
	```
	
- MTM_CommentsContribution_num -
	```
	[Measures].[m_CommentsContribution] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CommentsContribution])
	```

- MTM_CommentsContribution_percent -
	```
	Divide(([Measures].[m_CommentsContribution] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CommentsContribution])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CommentsContribution]), 0)
	```

- MTM_Posts_num - Monthly growth of posts created by the user.
	```
	[Measures].[m_ID_distinct] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_ID_distinct])
	```

- MTM_Posts_percent - Monthly growth of posts created by the user as a percentage.
	```
	Divide(([Measures].[m_ID_distinct] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_ID_distinct])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_ID_distinct]), 0)
	```

- MTM_Questions_num - Monthly growth of posts with type Questions.
	```
	[Measures].[m_CountQuestions_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountQuestions_sum])
	```

- MTM_Questions_percent - Monthly growth of posts with type Questions in percentage.
	```
	Divide(([Measures].[m_CountQuestions_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountQuestions_sum])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountQuestions_sum]), 0)
	```

- TotalArticlesMonth - The cumulative total of posts with type Article.
	```
	Sum([DateDimensionPost].[DateDimensionPost].CurrentMember.FirstChild : 
		[DateDimensionPost].[DateDimensionPost].CurrentMember,[Measures].[m_CountArticle_sum])
	```
	
- TotalPostsMonth - Cumulative total of posts.
	```
	Sum([DateDimensionPost].[DateDimensionPost].CurrentMember.FirstChild : 
		[DateDimensionPost].[DateDimensionPost].CurrentMember,[Measures].[m_ID_distinct])
	```

- TotalQuestionsMonth - The cumulative total of posts with type Questions.
	```
	Sum([DateDimensionPost].[DateDimensionPost].CurrentMember.FirstChild : 
		[DateDimensionPost].[DateDimensionPost].CurrentMember,[Measures].[m_CountQuestions_sum])
	```

- YTY_Articles_num - Yearly growth of posts with type Article created by the user.
	```
	[Measures].[m_CountArticle_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CountArticle_sum])
	```

- YTY_Articles_percent - Yearly growth in percentage of posts with type Article created by the user.
	```
	Divide(([Measures].[m_CountArticle_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CountArticle_sum])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CountArticle_sum]), 0)
	```

- YTY_CommentsContribution_num -
	```
	[Measures].[m_CommentsContribution] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CommentsContribution])
	```

- YTY_CommentsContribution_percent -
	```
	Divide(([Measures].[m_CommentsContribution] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CommentsContribution])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CommentsContribution]), 0)
	```

- YTY_Posts_num - Yearly growth of posts created by the user.
	```
	[Measures].[m_ID_distinct] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_ID_distinct])
	```

- YTY_Posts_percent - Yearly growth in percentage of posts created by the user.
	```
	Divide(([Measures].[m_ID_distinct] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_ID_distinct])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_ID_distinct]), 0)
	```

- YTY_Questions_num - Yearly growth of posts with type Question created by the user.
	```
	[Measures].[m_CountQuestions_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.Lag(12), [Measures].[m_CountQuestions_sum])
	```

- YTY_Questions_percent - Yearly growth in percentage of posts with type Question created by the user.
	```
	Divide(([Measures].[m_CountQuestions_sum] - 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountQuestions_sum])), 
		([DateDimensionPost].[DateDimensionPost].CurrentMember.PrevMember,[Measures].[m_CountQuestions_sum]), 0)
	```
 
### Formula field
- CommentsPerDay - Average number of comments per month.
	```
	@m_CommentsContribution / 365 * 12
	```

- Lang - Returns an array where the first value is the community region and the second value is a link to community.intersystems. The region depends on the first two-character filename EnCommunityAnalytics.cls = English Region. 
	```
	String lang = UpperCase(Left(reportName(), 2));

	if (lang == "EN"){ return ["English", "https://community.intersystems.com/"] }
	else if (lang == "PT"){ return ["Portuguese", "https://pt.community.intersystems.com/"] }
	else if (lang == "CN"){ return ["Chinese", "https://cn.community.intersystems.com/"] }
	else if (lang == "ES"){ return ["Spanish", "https://es.community.intersystems.com/"] }
	else if (lang == "JP"){ return ["Japanese", "https://jp.community.intersystems.com/"] }
	else if (lang == "FR"){ return ["French", "https://fr.community.intersystems.com/"] }
	```

- Last12Month - Date 12 months ago in YYYYMM format.
	```
	return (Year() - 1) * 100 + Month()
	```
	
- Last1Month - Date months ago in YYYYMM format.
	```
	Date a = FirstDayOfMonth(@CurrentDate) - 1;
	return (Year(a) * 100) + Month(a)
	```
	
- Last2Month - Date 2 months ago in YYYYMM format.
	```
	Date a = FirstDayOfMonth(@CurrentDate) - 32;
	return (Year(a) * 100) + Month(a)
	```
	
- Last6Month - Date 6 months ago in YYYYMM format.
	```
	Date d = FirstDayOfMonth(@CurrentDate) - 180;
	return (Year(d) * 100) + Month(d)
	```
	
- LastYearStr - Date 12 months ago in Mon-YYYY format.
	```
	return ToText(ToDate(Year() - 1, Month(), 1), 'MMM-YYYY')
	```
	
- PostsPerDay - Average number of posts per month.
	```
	@m_ID_distinct / 365 * 12
	```
	
- YearWeek - Date in WWwYYYY format.
	```
	ReplaceString(ToText(@YearPosts), ',', '') + 'W' + ToText(@WeekYear)
	```
	
- PostType_label - Formula for defining custom labels in charts.
	```
	if(@PostType == "ARTICLE"){ return "New Articles"}
	else if(@PostType == "QUESTION"){ return "New Qeustions" }
	else if(@PostType == "ANNOUNCEMENT"){ return "New Announcements" }
	else if(@PostType == "DISCUSSION"){ return "New Discussions" }
	```
	
- CurrentDate - full current date.
	```
	Today()
	```

- CurrentMonth - current month date in MMMYYYY format.
	```
	Date a = FirstDayOfMonth(@CurrentDate) 
	return (Year(a) * 100) + Month(a)
	```
   
- Each Queries has a filter that excludes the current month

  #### Posts
	```
	@MonthYearNum < @CurrentMonth
	```

  #### Members
	```
	@members_MonthYearNum < @CurrentMonth
	```

  #### ContributedMembers
	```
	@contributedmembers_MonthYearNum < @CurrentMonth
	```

  #### Views
	```
	@views_MonthYearNum < @CurrentMonth
	```
</details>

## General information

A member counts in every community in which (s)he participates. All members participate in the English community by default. If a member has shown any activity in a non-English community, we mark him or her as a new member using the date of registration. So, if community in French was launched in March we can still see members joined in January and previous months. Members who have been banned from the portal do not count.

Removed data about unpublished and deleted posts, as well as Digest type posts from data about posts and their views.

## Page 2

### Members Total table
_The table shows the dynamics of the number of members, new members per month, new contributed members per month._ 

<details>
<summary>For developers</summary>

#### Measures
- MemberMonthTotal
- m_ID_distinct
- m_Member_distinct
	
#### Dimensions
- member_MonthYear

#### Filters
- members_MonthYearNum >= @Last6Month

</details>

### Members Total bar chart
_Members Total shows the number of users registered on the site at the end of each month._ 

<details>
<summary>For developers</summary>

#### Measures
- TotalMemberMonth
	
#### Dimensions
- members_MonthYear

#### Filters
- members_MonthYearNum >= @Last6Month

</details>

### Post Total table
_The table shows the cumulative total and the number of new posts per month by Post type (Questions, Article, Announcement, Discussion)_ 

<details>
<summary>For developers</summary>


#### Measures
- TotalPostsMonth
- TotalQuestionsMonth
- TotalArticlesMonth
- m_ID_distinct
- m_CountQuestions_sum
- m_CountArticle_sum
- m_CountAnnouncement_sum
- m_CountDiscussion_sum

#### Dimensions
- MonthYear

#### Filters
- MonthYearNum >= @Last6Month

</details>

### Post Total bar chart
_This chart shows the number of new posts by type for each month._ 

<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct Group By PostType
	
#### Category (X-Axis)
- PostType

#### Dimensions (Series)
- MonthYear

#### Filters
- MonthYearNum >= @Last6Month
	
Bar chart with New Questions, New Articles, New Announcements and New Discussions in Show Values, MonthYearPosts in Category. Only the last 6 months are taken into MonthYearPosts using the Select Bottom N function.

</details>

## Page 3

### View Total table
_The table shows the total number of post views, new views per month, the average number of Article views, and the average number of Questions views. Showing the last 6 months._  

<details>
<summary>For developers</summary>
	
#### Rows
- views_PostType

#### Columns
- views_MonthYear

#### Measures
- TotalViewsMonth
- m_Delta_sum

#### Crosstab Formulas
- AvgViewsArticle
	```
	@(@views_PostType:"ARTICLE", Sum(@m_Views_avg))
	```

- AvgViewsQuestion
	```
	@(@views_PostType:"QUESTION", Sum(@m_Views_avg))
	```

#### Filters
- views_MonthYearNum >= @Last6Month
	
</details>

### View Total bar chart
_The chart shows the number of new views of all community posts per month._ 

<details>
<summary>For developers</summary>

#### Dimensions
- views_MonthYear

#### Measures
- m_Delta_sum
	
#### Filter
- views_MonthYearNum >= @Last6Month
	
Bar chart with m_Delta_sum in Show Values and MonthYearView in Category. Only the last 6 months are taken into MonthYearView using the Select Bottom N function.

</details>

## Page 4

### Members Base bar chart

_This diagram shows the breakdown of all new members per month into 2 categories - InterSystems employees and customers._ 

<details>
<summary>For developers</summary>


#### Measures
- members_m_ID_distinct Group By members_ISCMemberStr
	
#### Category (X-Axis)
- members_ISCMemberStr

#### Series
- members_MonthYear
	
#### Filters
- members_MonthYearNum >= @Last12Month

</details>


### Month-to-Month Member Growth

_This table shows a comparison with the previous month in terms of the number of new users._

<details>
<summary>For developers</summary>
	
#### Measures
- TotalMemberMonth
- MTM_Member_num
- MTM_Member_percent

#### Rows
- members_MonthYear

#### Columns
- members_ISCMemberStr

#### Filters
	```
	members_MonthYearNum = @Last1Month
	OR
	members_MonthYearNum = @Last2Month
	```

</details>

### Contributors Monthly bar chart

_This chart shows the number of active members per month._

<details>
<summary>For developers</summary>
	
#### Measures
- contributedmembers_MonthYear
	
#### Dimensions
- m_Member_distinct

#### Filters
- members_MonthYearNum >= @Last12Month
	
</details>


## Page 5

### Customers Contributors Monthly bar chart

_This chart shows the number of active customers per month._

<details>
<summary>For developers</summary>

#### Measures
- m_Member_distinct

#### Dimensions
- contributedmembers_MonthYear

#### Filters
	```
	contributedmembers_ISCMembersStr = Customers
	AND
	contributedmembers_MonthYearNum >= @Last12Month
	```
	
</details>

### InterSystems Contributors Monthly bar chart

_This chart shows the number of active InterSystems employees per month._


<details>
<summary>For developers</summary>

#### Measures
- m_Member_distinct

#### Dimensions
- contributedmembers_MonthYear

#### Filters
	```
	contributedmembers_ISCMembersStr = InterSystems
	AND
	contributedmembers_MonthYearNum >= @Last12Month
	```
	
</details>


### Posting Contribution

_This chart shows the number of new posts and new comments on all posts per month._


<details>
<summary>For developers</summary>

#### Measures
- m_CommentsContribution Group By MonthYear
- m_ID_distinct Group By MonthYear
	
#### Dimensions
- MonthYear

#### Filters
- MonthYearNum >= @Last12Month
	
</details>


## Page 6

### Postings by Type

_This chart shows a breakdown of the number of new posts per month by post type._ 


<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct Group By PostType

#### Category (X-Axis)
- PostType
	
#### Series
- MonthYear

#### Filters
- MonthYearNum >= @Last12Month
	
</details>


### Products Contribution Breakdown: Caché, Ensemble, HealthShare, IRIS

_This chart shows a breakdown of the number of new posts per month by project._

<details>
<summary>For developers</summary>

#### Measures
- m_CountTagCache_sum Group By MonthYear
- m_CountTagEnsemble_sum Group By MonthYear
- m_CountTagHealthShare_sum Group By MonthYear
- m_CountTagIRIS_sum Group By MonthYear

#### Dimensions
- MonthYear

#### Filters
- MonthYearNum >= @Last12Month

</details>

### Articles by InterSystems And Customers breakdown

_This chart shows a breakdown of the number of new Articles per month by author status: InterSystems employee or customer._ 

<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct Group By ISCMemberStr

#### Category (X-Axis)
- ISCMemberStr

#### Series
- MonthYear

#### Filters
```
MonthYearNum >= @Last12Month
AND
PostType = Article
```
	
</details>

## Page 7

### Questions by InterSystems and Customers Breakdown

_This chart shows a breakdown of the number of new Questions per month by author status: InterSystems employee or customer._ 


<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct Group By ISCMemberStr

#### Category (X-Axis)
- ISCMemberStr

#### Series
- MonthYear

#### Filters
```
MonthYearNum >= @Last12Month
AND
PostType = Question
```
	
</details>


### Customers Weekly Contribution Level in Questions

_This chart shows the number of new Questions per week._


<details>
<summary>For developers</summary>

#### Measures
- m_CountQestions_sum
	
#### Category (X-Axis)
- YearWeek

#### Filters
- MonthYearNum >= @Last12Month
	
</details>


### Comments and Answers

_This chart shows a breakdown of the number of new Comments on all posts per month by author status: InterSystems employee or customer._


<details>
<summary>For developers</summary>

#### Measures
- m_CommentsContribution Group By posts_ISCMemberStr

#### Category (X-Axis)
- posts_ISCMemberStr

#### Series
- MonthYear
	
#### Filters
- MonthYearNum >= @Last12Month

</details>


## Page 8

### Contribution

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day._

<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct
- m_CountArticle_sum
- m_CountQuestions_sum
- m_CommentsContribution
- PostsPerDay
- CommentsPerDay
- MTM_Posts_num
- MTM_Posts_percent
- MTM_Articles_num
- MTM_Articles_percent
- MTM_Questions_num
- MTM_Questions_percent
- MTM_CommentsContribution_num
- MTM_CommentsContribution_percent
- YTY_Posts_num
- YTY_Posts_percent
- YTY_Articles_num
- YTY_Articles_percent
- YTY_Questions_num
- YTY_Questions_percent
- YTY_CommentsContribution_num
- YTY_CommentsContribution_percent
	
#### Rows
- MonthYear
	
#### Crosstab Formulas
- CommentsPerDay
	```
	(SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)) / 365 * 12
	```
	
- CommentsPerDay_MTM
	```
	SUM(@MTM_CommentsContribution_num) / 365 * 12
	```
	
- CommentsPerDay_YTY
	```
	SUM(@YTY_CommentsContribution_num) / 365 * 12
	```

- LastYearArticles
	```
	SUM(@m_CountArticle_sum) - SUM(@YTY_Articles_num)
	```

- LastYearComments
	```
	SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)
	```

- LastYearPosts
	```
	SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)
	```

- LastYearQuestions
	```
	SUM(@m_CountQuestions_sum) - SUM(@YTY_Questions_num)
	```

- PostsPerDay
	```
	(SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)) / 365 * 12
	```
	
- PostsPerDay_MTM
	```
	SUM(@MTM_Posts_num) / 365 * 12
	```

- PostsPerDay_YTY
	```
	SUM(@YTY_Posts_num) / 365 * 12
	```

#### Formula
- Average(m_ID_distinct)
- Average(m_CountArticle_sum)
- Average(m_CountQuestions_sum)
- Average(m_CommentsContribution)
- Average(PostsPerDay)
- Average(CommentsPerDay)

#### Filters
- MonthYearNum >= @Last2Month
	
</details>

### InterSystems

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day. Only data on the activity of InterSystems employees was taken._

<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct
- m_CountArticle_sum
- m_CountQuestions_sum
- m_CommentsContribution
- PostsPerDay
- CommentsPerDay
- MTM_Posts_num
- MTM_Posts_percent
- MTM_Articles_num
- MTM_Articles_percent
- MTM_Questions_num
- MTM_Questions_percent
- MTM_CommentsContribution_num
- MTM_CommentsContribution_percent
- YTY_Posts_num
- YTY_Posts_percent
- YTY_Articles_num
- YTY_Articles_percent
- YTY_Questions_num
- YTY_Questions_percent
- YTY_CommentsContribution_num
- YTY_CommentsContribution_percent
	
#### Rows
- MonthYear
	
#### Crosstab Formulas
- CommentsPerDay
	```
	(SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)) / 365 * 12
	```
	
- CommentsPerDay_MTM
	```
	SUM(@MTM_CommentsContribution_num) / 365 * 12
	```
	
- CommentsPerDay_YTY
	```
	SUM(@YTY_CommentsContribution_num) / 365 * 12
	```

- LastYearArticles
	```
	SUM(@m_CountArticle_sum) - SUM(@YTY_Articles_num)
	```

- LastYearComments
	```
	SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)
	```

- LastYearPosts
	```
	SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)
	```

- LastYearQuestions
	```
	SUM(@m_CountQuestions_sum) - SUM(@YTY_Questions_num)
	```

- PostsPerDay
	```
	(SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)) / 365 * 12
	```
	
- PostsPerDay_MTM
	```
	SUM(@MTM_Posts_num) / 365 * 12
	```

- PostsPerDay_YTY
	```
	SUM(@YTY_Posts_num) / 365 * 12
	```

#### Formula
- Average(m_ID_distinct)
- Average(m_CountArticle_sum)
- Average(m_CountQuestions_sum)
- Average(m_CommentsContribution)
- Average(PostsPerDay)
- Average(CommentsPerDay)

#### Filters
```
MonthYearNum >= @Last2Month
AND
posts_ISCMembersStr = InterSystems
```
	
</details>

### Customers

_The table shows MTM and YTY comparisons with the current month. The comparison is based on data on all posts, articles, questions and comments, as well as on the average number of posts and comments per day. Only data on the activity of customers was taken._

<details>
<summary>For developers</summary>

#### Measures
- m_ID_distinct
- m_CountArticle_sum
- m_CountQuestions_sum
- m_CommentsContribution
- PostsPerDay
- CommentsPerDay
- MTM_Posts_num
- MTM_Posts_percent
- MTM_Articles_num
- MTM_Articles_percent
- MTM_Questions_num
- MTM_Questions_percent
- MTM_CommentsContribution_num
- MTM_CommentsContribution_percent
- YTY_Posts_num
- YTY_Posts_percent
- YTY_Articles_num
- YTY_Articles_percent
- YTY_Questions_num
- YTY_Questions_percent
- YTY_CommentsContribution_num
- YTY_CommentsContribution_percent
	
#### Rows
- MonthYear
	
#### Crosstab Formulas
- CommentsPerDay
	```
	(SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)) / 365 * 12
	```
	
- CommentsPerDay_MTM
	```
	SUM(@MTM_CommentsContribution_num) / 365 * 12
	```
	
- CommentsPerDay_YTY
	```
	SUM(@YTY_CommentsContribution_num) / 365 * 12
	```

- LastYearArticles
	```
	SUM(@m_CountArticle_sum) - SUM(@YTY_Articles_num)
	```

- LastYearComments
	```
	SUM(@m_CommentsContribution) - SUM(@YTY_CommentsContribution_num)
	```

- LastYearPosts
	```
	SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)
	```

- LastYearQuestions
	```
	SUM(@m_CountQuestions_sum) - SUM(@YTY_Questions_num)
	```

- PostsPerDay
	```
	(SUM(@m_ID_distinct) - SUM(@YTY_Posts_num)) / 365 * 12
	```
	
- PostsPerDay_MTM
	```
	SUM(@MTM_Posts_num) / 365 * 12
	```

- PostsPerDay_YTY
	```
	SUM(@YTY_Posts_num) / 365 * 12
	```

#### Formula
- Average(m_ID_distinct)
- Average(m_CountArticle_sum)
- Average(m_CountQuestions_sum)
- Average(m_CommentsContribution)
- Average(PostsPerDay)
- Average(CommentsPerDay)

#### Filters
```
MonthYearNum >= @Last2Month
AND
posts_ISCMembersStr = Customers
```
	
</details>

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

### Customers

_This table shows the most active community members of all time._

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
