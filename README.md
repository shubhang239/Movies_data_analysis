## Movies Data Analysis
### ⭐ Abstract
Data Analysis was performed on the <a href="https://www.kaggle.com/tmdb/tmdb-movie-metadata">Tmdb Movies Dataset</a> which contains data related to over 5000 movies. 
Various facets of an analytics project like cleaning and processing the data, extracting relevant information, changing data types, feature engineering etc was carried out. 
An outlier analysis was also done using box plots and violin plots to filter the extreme and incorrect values. The analysis carried out involved finding descriptive statistics, distribution of the data and patterns in the data. We gathered various insights and visualized them using a variety of plots such as bar plots, line charts and scatter plots. <b>Excel</b> was used for preprocessing and understanding, <b>Python 3.0</b> was used for most of the analysis and hypothesis testing was done in <b>R</b>. 
Several important conclusions were drawn such as the Run time of the movie does not affect the revenue generated. Higher budget films tend to be more popular but do not necessarily generate more revenue.
### 🌱 Experimental Design
#### Data Cleaning
The first step was Data Pre-processing which includes Data Cleaning, Data Normalization/ Standardization etc. The dataset contained many null values which were dropped.

#### Null Values Imputation
The columns which contained over 60 % Null values in them were dropped. The main challenge here wasn't just missing values but the presence of wrong values which can entirely mess up the analysis. These are highlighted below:
- Handling the 0 values in Budget and Revenue Columns
<img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image2.png" height=200px alt=""/>
As we can see our data contains a lot of zeros in columns like budget, revenue, vote_average etc. This could be mainly due to two reasons;
a) Improper Data Collection or
b) The movie was never made.
Regarding this I looked up on the internet about the movies and found that most of them were released and had a sizable budget. Thus this points towards faulty or incomplete data collection. One way to handle this could be to:
<li><b>Drop the rows with zero values of budget</b>, but this is a rather crude way to handle the problem and since the number of rows having zero are large, it would lead to a loss of data present in other columns. Another approach can be to:</li>
<li><b>Replace the zeroes with either the mean, mode, median etc.</b> However if you think again, it is not such a good idea to replace with measures of central tendency as the budget depends on a large number of factors and varies significantly case to case.</li>
<li><b>Using a Machine Learning model to predict the values.</b> The Problem with this technique is that we don't have enough data points to build a reliable model and also the existing data is unclean and has a lot of missing/ wrong values. Further it would take a lot of time to build a model whose accuracy will not even be that good.</li>
<li><b>Replacing the 0's with np.NaN or null values.</b> This is the overall best approach from our analysis point of view. Since we are interested in Exploratory Data Analysis which can tell us the trends or patterns its best we dont impute any of our own values inplace of zero, so as to not change the true underlying pattern present in the data.</li>

#### Univariate Numerical Columns Analysis
As a next step we performed Univariate Analysis of every numerical feature and visualized its distribution, normality etc.

<table>
  <tr>
    <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image10.png" height=200px alt=""/><br /><sub><b>Popularity</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image11.png" height=200px alt=""/><br /><sub><b>Vote Average</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image12.png" height=200px alt=""/><br /><sub><b>Revenue</b></sub><br /></td>
  </tr>
   <tr>
    <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image4.png" height=200px alt=""/><br /><sub><b>Run Time</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image15.png" height=200px alt=""/><br /><sub><b>Vote Count</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image17.png" height=200px alt=""/><br /><sub><b>Budget</b></sub><br /></td>
  </tr>
 </table>
 
#### Outlier Analysis

<table>
  <tr>
    <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image7.png" height=200px alt=""/><br /><sub><b>Violin Plot</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image8.png" height=200px alt=""/><br /><sub><b>Box Plot</b></sub><br /></td>
  </tr>
 </table>
    
#### Feature Engineering
Columns such as genres, keywords, production companies contain multiple values in JSON format (which looks like a list of dictionaries but it is actually a string type. To extract useful values out of them we will apply techniques to extract relevant features.
These special columns are:
<li><b>genres</b>: It's in JSON format or a list of dictionaries in python. It has two key fields, 'id' and 'name' where id refers to each type of genre such as comedy, action , romance etc. We need to extract only the genre name out of it. Many films fall under multiple genres hence we will get more than one name in these cases.</li>
<li><b>production_companies</b>: This column contains information in an JSON format and contains a 'id' and 'name' key for companies which have produced the film. In some cases there are multiple companies which seem to have produced a single movie. I was not too sure about this so I looked it up and found that it is indeed true. For example the movie Avatar has been produced by 20th Century Fox, Lightstorm Entertainment, Dune Entertainment and Ingenious Film Partners.</li>
<li><b>production_countries</b>: It is also in JSON format. The 'ISO id' key is the abbreviation of the country. The 'name' country contains the name of the countries where the film scenes were produced.</li>
<li><b>release_date</b>: This column is a date-time object and needs to be converted using the datetime function.
We will also create a new column called total_votes which is a product of vote_average and vote_count. We have to do this because the number of people who voted for different movies is different, hence by only analysing one column we won't get a complete picture.</li>

#### Bi-Variate Analysis
Plotted Scatter Plots between Variables of interest to indentify trends and relations. We also identified the linear correlation between various features in the dataset in order to see the increase-decrease together.

<table>
  <tr>
    <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image9.png" height=200px alt=""/><br /><sub><b>Violin Plot</b></sub><br /></td>
    <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image14.png" height=200px alt=""/><br /><sub><b>Violin Plot</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image16.png" height=200px alt=""/><br /><sub><b>Box Plot</b></sub><br /></td>
  </tr>
 </table>
 
 #### Visualizations
 Plotted graphs to identify results and present them in appealing manner.
 <table>
  <tr>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image1.png" height=200px alt=""/><br /><sub><b>Top 10 Most popular Movies</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image3.png" height=200px alt=""/><br /><sub><b>Top 10 Highest Grossing Films</b></sub><br /></td>
  </tr>
  <tr>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image5.png" height=220px width = 420px alt=""/><br /><sub><b>Top 10 Longest Movies by Runtime</b></sub><br /></td>
     <td align="center"><img src="https://github.com/shubhang239/Movies_data_analysis/blob/cbc9d69b90fa9b5186902d18a8d91fccbae3b93f/images/image6.png" height=220px width=420px alt=""/><br /><sub><b>Top 10 Movies by Total Votes</b></sub><br /></td>
  </tr>
 </table>
