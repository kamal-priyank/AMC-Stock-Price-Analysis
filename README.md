***AMC Stock Analysis***

We are taking the position of an investment company that saw the shares of AMC Entertainment soar at the end of May 2021. We believe that the main cause of the sudden rise in AMC’s stock price is due to the traders on social media platforms encouraging people to buy the stock. As AMC is one of the most talked about stock on Reddit’s 10-million-member Reddit group, WallStreetBets, we would like to focus mainly on Reddit in this paper and to understand how the users of the platform hyped the price to unprecedented levels as they believe that short selling is part of a rigged game. The question we form for this analysis, is as following: *Did AMC’s stock go from being unloved on WallStreet to becoming a favorite with the help of Reddit traders?* 

The data required to answer our formulated question has been downloaded from the Nasdaq Stock Market webpage. It is a dataset for the AMC Entertainment Holdings stock price history. (nasdaq.com, 2021) For the relevance of the analysis, we will be looking at the period of January to September 2021, as this is the period stock price experienced huge shifts. Moreover, we required further information about how Reddit users influenced the stock price. To do so, we used the Reddit API and Google trends to look at how people associated with AMC in the given period we are interested in. 

***Data scraping***

After collecting the stock price data, we started by using the Google API. We identified the timeframe to look at the interest over time for AMC. It was no surprise to see that the number of searches was much higher in February and end of May, respectively when the stock price experienced an increase. However, in the time before and after the search level was relatively low. 

Next, to pull data from Reddit, we used its API. Originally, we only wanted to look at the subreddit “WallstreetBets” to limit the posts to those where users were specifically talking about the stock. However, here we faced the challenge of not being able extract enough posts that would match with the dates for the rise in stock prices. To overcome this, we decided to instead look at all subreddits that had “amc” in its title. Although now, we did not know if all the posts we scraped were related to the stock or not, but they were taken from the “hot” section for this year which creates relevance for the analysis. 

Moving on, we wanted to look at the comments under “WallstreetBets” related to amc. At first, we got a “MoreComments” object, so we used “replace\_more()” in our code to get rid of this and to load more comments. We received a lot of comments, and scrolling through the window, we could see many interesting discussions regarding the stock of AMC. Moreover, we also scraped posts with “amc” in their title to perform a sentiment analysis. Although we have a very high percentage of neutral posts, there are more positive than negative. Moreover, in this case we were interested in looking at the user’s association to AMC more than its correlation to stock price. To fully utilize the comments and sentiments, further text analysis would be required to identify posts that primarily discuss the stock price.

***Data Cleaning***

As the first steps we started with checking for duplicate values in the merged data frame using the functions duplicated () and any (), which returns the Boolean value True or False, we found that there are no duplicates. So, we move to the next step which is to check if there are any NA values present in the data frame using the .isna() function, if present may result in errors while performing analysis on the data accumulated, we realize that there are no NA values present in any of the columns as it returns a Boolean value False for all the columns. Now we move on to check for the unique values in each column and if a column has only a single unique value, we drop that column from the data frame using the .drop() function, we compute that there are no columns with only a single unique value.

***Data Scaling***

For this part, we dropped all the columns that are not computable mathematically using the drop () function, which are “title”,” date”,” time” and Date” namely. We then defined a list of columns from which the” $” symbol is to replace empty strings to make the values of those columns computable, this is done by using the replace function with setting the regex argument to “True” and then then is given to a new data frame Scaled\_df ,which is then used further to bring out the scaled results using the function preprocessing. scale ().

***Data pre-processing***

Data imputation is used to compensate for the missing values in a dataset by calculating the mean or median for the existing values in the data frame and replacing either one of them with the np.nan I.e., empty values found within the data frame being cleaned.

To find the missing values and replace them with mean value of the corresponding column is done using the impute.SimpleImputer() which initiates the imputer and then a fit () and a transform () function are used to fit the imputer initiated and to transform all the missing values in the given data frame.

As we are unlikely to have any missing values in the data frame, it is highly possible that there is no need for imputation to be performed.

Alternatively, we can also choose to drop any rows which have NA values using .dropna() function, which is what is done to the merged data.

***Outliers***

To visually check If there were any outliers present in the scraped data frame, we generated a boxplot using seaborn, giving number of comments per post on a particular day to the x-axis and the Volume of stocks traded in a day to the Y-axis, which revealed a few significant outliers present which could make the results of the analysis skewed.

In the presence of outliers, we had the entire rows with the outlier removed, by calculating the Z-score of each entry in the desired Column using the stats.zscore() function imported from the Scipy package that takes the column of the data frame and nan\_policy as the arguments.

To ensure fair analysis, an element of the column having Z-score greater than 3 or less than –3 is considered as an outlier, which brings us to using. where () function with conditions mentioned to detect outliers, the array of rows which have the outliers are returned as we run the function. Lastly, the list of rows is simply removed using the .drop () function.

***Feature Engineering***

Under feature engineering we are required to add more features to the data frame by computing values from the existing columns, here we applied feature transforms to convert certain columns into log values using the.apply() function giving in argument np.log as the mathematical function to be applied.

To avoid confusion, we create a separate data frame by copying the scaled\_df  of which the log values of the Volume column are calculated because the Volume of stock traded is given in millions per day which may alter the graphical representation. Moreover, it made our data more normally distributed for the values of Volume after converting to its log. 

***Lexicons***
<p align = "center">
<img width="684" alt="Screenshot 2022-10-02 at 22 07 11" src="https://user-images.githubusercontent.com/66077662/193476294-da945f10-2b18-41f5-a48d-37b0b3820051.png">
</p>
<h5 align="center">Variable Description</h5>

<p align="center">
<img width="693" alt="Screenshot 2022-10-02 at 22 08 09" src="https://user-images.githubusercontent.com/66077662/193476335-de317c9e-744c-4fcb-8ff2-5297db1b7d8a.png">

</p>
<h5 align="center">Mean, Standard Deviation, Median, Minimum and Maximum</h5>

***Summary statistics***

In this section, we used the variables we computed from the above sections to run a regression model. Firstly, by converting the Volume to its log, we got a much more normally distributed data for this variable. Moving on, we plotted our variables into a correlation plot. A strong correlation amongst the variables scraped from Reddit was already presumed, however, we also saw some correlation in volume of trade and- number of comments, rolling average and searches for instance. This indicates that some of the variables do play a role in affected the stock price and the volume of trade. 

Lastly, we formed a hypothesis test and ran a test using the OLS method. We assigned Log(Volume) as the dependent variable and tested it against 'search', 'upvote ratio', 'score', and 'number of comments' variables. 

H0 = There is no relationship between the dependent- and the independent variables

HA = There is a relationship between the dependent- and independent variables. 

By looking at the results of our regression, we see that all variables besides *number of comments* are significant. With a p-value beyond 5%, we can remove it from the model and run the test again. The R2 value we receive is 0.762 which indicates that it can explain almost 76% of the variability in our data. We have calculated a significant model to predict the log value of Volume of trade for the stock of AMC. This allows us to say that we can in some terms determine that Reddit had a great positive influence on the trade- and in turn on the stock price of AMC Entertainment. As we know, when there is more demand than supply for a stock, the price is bound to shoot. 

***Additional data***

|**Additional info/data**|**Analytical tools**|
| :- | :- |
|- Further analyze the comment section under the subreddit “WallstreetBets” and look at the structure of the comment tree. |<p>**Reddit API**</p><p></p><p></p>|
|<p>- A sentiment analysis can then be performed on these comments to understand what the feelings of these Reddit users were when discussing the AMC stock. </p><p></p>|<p>**Reddit API**</p><p>**+**</p><p>**Sentiment Analyzer (from nltk library in Python)**</p>|
|- Financial statements can be used to compute financial ratios that indicate the health or value of AMC and its shares.||
|<p>- Can also look at relationship between Reddit and Twitter </p><p>- Did Reddit start the trend, caught on by other social media platforms? </p>|- **Use Twitter API (Many trending hashtags related to amc stock)**|

