#1.DATA PREPERATION

##1.1 STRUCTUR THE COLUMNS
 
The structure of initial data does not make much sense. There are few issues that catches attention on the surface:
1.	The column headings do not make much sense.
2.	The first row of the dataframe is not an observation rather is the options for the provided questions.

Initial browsing through the data reveals a pattern that a column containing a question is followed by their available responses as Unnamed, but the actual options for that question is present in the 1st row of dataset. Also, using the question and options provided as it is will not be appropriate for analysis due to their large size.

Solution: To tackle the above stated problem, I renamed all the columns in a concise way. To control any information loss due to our imputation and to facilitate use of original information later in the analysis I prepared a dictionary.

To structure the columns properly I removed the 1st row with index [0] and preserved the options in a dictionary called options.
   Result

##1.2 UNNECESSARY OBSERVATIONS

Going a little bit further in my analysis I noticed a trend that any respondent who have not answered the Q2 have not answered rest of the questions as well, so all the columns contains NaN.
  

Solution: A straight up elimination of all these observations were done as they can’t provide any meaningful information in our analysis. But I am going to keep respondents who have answered Q1(Have you seen any of 6 films…) as No because doing so makes sense in two ways:
1.	Respondents who have not watched any movies are going to skip all answers about movies.
2.	They can provide valuable demographic information and can provide interesting insights.

Before going any further in our analysis, I took a step and made a subset of responses containing Q1 as Yes and named new dataframe as df_yes. As stated above responses with Q2 as NaN does not contain any other information except demographics. Rest of the responses is still kept in original dataframe to be used in further analysis.

##1.3 WHITESPACES

I took a strategy of scanning the data column wise to look for potential errors and problems. Only column Q1 came out as containing whitespaces in some of its observation. With values  
‘Yes’ and ‘Yes’ both present in the same column.

Solution: A simple str.strip() took out all the unnecessary whitespaces from the column.

##1.4 TYPOS

Q2, Q8 ,Q9 and Gender columns had typographical errors which was creating an issue  of                   .  Solution: I used a simple method in all of these cases taking example of of the above stated problem in column Q9. I defined a dictionary with key as typos and values as the desired outcome of typos and used it with a .replce() for example : df_yes['Q9'].replace({'yes':'Yes','Noo':'No'})

##1.5 CHANGING DATATYPES

a)	Q3/O1-Q3/O6 contains responses for Q3(which addresses wether or not respondent have watched the particular movie column is representing) contains name of the movie if the respondent answered yes and a NaN otherwise.
b)	Q4/O1-Q4/O6 contains ratings for different movies of the franchise but datatype of each column comes up as object which is needed to be changed to int. 

Solution: To address 1st problem I and to make data more presentable I wrote a function impute_val to impute 1 in place of a yes response and 0 otherwise. To address the 2nd issue .astype(int) is used.

##1.6 SANITY CHECK

While analyzing Age column an observation came up which contained an observation of Age as 500

Solution: Since, from all the possible solutions to treat this extreme value like imputing it with a mode(as column is categoric) removing it altogether as it provides misleading information was a logical choice.


##1.7 MISSING VALUE

Two different approaches were implemented to treat missing values at two different situation.

1.	Manual imputation: In Q4/O3 and Q4/O1 columns 1 missing value was found in each of them.
  
From close observation it comes out that the missing value should be remaning 1 rating which in this case is 6.

2.	Predicting missing value: In pursuit to free our dataset from all null values when I checked null values in all the columns Household Income has a significant amount of NaN about 161 values. Replacing these values with mode or simply removing these observations can either lead to a misrepresentation of observation or loss in information from our dataset. So, a logical decision of using a classification prediction model was made to predict these absent observations.
This whole process was done in some simple steps:
1.	Subsetting data in 2 separate dataset one containing observation where household income was NaN and one where all the data is present which will be used in building model.
2.	Substituting all the nominal data with dummy variable and ordinal data with integer encoding.
3.	Fitting a decision tree with our processed data and use cross validation to tune hyperparameters for the model.
4.	Predicting missing data and substituting it back to main dataframe.

After imputing all the predicted value back to dataframe I am going to drop all the remaining observations containing null in any column.

Conclusion:
After doing all our pre-processing the original dataset reduced by a 16.76% and what’s left is a clean dataset.


#3.	DATA EXPLORATION










 
Figure1:average rating of movie

##2.1 EXPLORING RATING OF MOVIES

Fig 1 shows average rating of movies in the franchise and an increasing trend can be clearly seen till the 3rd movie of the franchise after which ratings started improving drastically. Fig 2 corroborate the first conclusion as it can be clearly seen that the 5th movie in the franchise received most number of top rating.

##2.2 RELATIONSHIP BETWEEN COLUMN
1.	Ho: The distribution of respondents who have watch or have not watched movies from Star Wars franchise is same among different genders.
Ha: The distribution of respondents who have watch or have not watched movies from Star Wars franchise is not same among different genders.






         
            











                
 
2.	
In my analysis Star Wars Episode III had the highest average rating so for this hypothesis test I am going build my hypothesis around it.

Ho: Different education levels have a difference in average rating for Star Wars Episode III.
Ha: Different education levels don’t have a difference in average rating for Star Wars Episode III.









3.	For my last hypothesis I am made a crossable to look for most popular Star Wars character in which Han Solo came up top with 549 vary favorable votes.

Ho: The distribution of respondents among different genders have same response towards Han Solo.
Ha: The distribution of respondents among different genders have different response towards Han Solo.

 
 

##2.3.	 RELATIONSHIP BETWEEN DEMOGRAPHICS AND CHARACTERS

To explore the relationship between demographics of people and their attitude towards characters in the franchise I had to take a different approach because to do so a large number of comparisons between various variables need to be done. 


Figure 1East nort central
 
Figure 2pacific


In my further analysis I found respondent with education level over a high school degree have a much better chance to find characters very favorable, below 1st image show result for respondents with bachelor’s degree followed by respondents with high school degree. 




Also, middle income ($50,000-$99,000) people found character as very favorably as compare to respondents with any other income level. Which is corroborated by the fact that about 40.8% of franchise followers lie in this income level.

