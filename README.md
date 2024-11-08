
# Project Title: Exploratory Data Analysis of [Dataset Name]

## Table of Contents
* [1. EDA Overview](#1-eda-overview)
* [2. Dataset Description](#2-dataset-description)
* [3. Task Goals](#3-task-goals)
* [4. Installation and Setup](#4-installation-and-setup)
* [5. Data Exploration](#5-data-exploration)
   - [Loading the Data](#loading-the-data)
   - [Initial Observations](#initial-observations)
* [Data Cleaning](#data-cleaning)
   - [Missing Values](#missing-values)
   - [Duplicates](#duplicates)
* [Analysis](#analysis)
   - [Overview of Dataset](#overview-of-dataset)
   - [Basic Descriptive Statistics](#basic-descriptive-statistics)
   - [Top Performers](#top-performers)
   - [Temporal Trends](#temporal-trends)
   - [Genre and Music Characteristics](#genre-and-music-characteristics)
   - [Platform Popularity](#platform-popularity)
   - [Advanced Analysis](#advanced-analysis)
* [Key Findings](#key-findings)
* [Conclusion](#conclusion)
* [Key Faced Errors](#key_faced_errors)
    
[References](#references)

[Timeline](#Timeline)

---

### 1. EDA Overview

This exploratory data analysis practices one's ability to code, clean data, visualize data, & data analysis. We are tasked to do the exploratory data analysis on the most streamed songs of Spotify 2023, which had source data anomalies, including wrong datatypes, missing values, and duplicated values. This is the reason why this dataset is suitable for data analysis, as it hones one's skill in various areas when executing the EDA.

---

### 2. Dataset Description
The origin of dataset comes from Kaggle, a famous data science competition platform and the online community of data scientists and machine learning practitioners under Google LLC.
  https://www.kaggle.com/dsv/6367938
  
This dataset contains a comprehensive list of the most famous songs of 2023, as listed on Spotify. The dataset offers many features beyond what is typically available in similar datasets. It provides insights into each song's attributes, popularity, and presence on various music platforms (Elgiriyewithana, 2023).

- **Features**: The dataset includes information such as
  
| Column                | Description                          |
|-----------------------|--------------------------------------|
| track_name            | Contains the title of the song          |
| artist(s)_name        | Contains the credited artists      |
| artist_count          | Contains the number of credited artists        |
| released_year         | Contains the track's release year     |
| released_month        | Contains the track's release month |
| released_day          |Contains the track's release day |
| in_spotify_playlists  | Contains the track's occurrences in Spotify Playlists  |
| in_spotify_charts     | Contains the track's occurrences in Spotify Charts   |
| streams               | Contains the track's number of streams |
| in_apple_playlists    | Contains the track's occurrences in Apple Music Playlists|
| in_apple_charts       | Contains the track's occurrences in Apple Music Charts |
| in_deezer_playlists   | Contains the track's occurrences in Deezer Playlists |
| in_deezer_charts      |Contains the track's occurrences in Deezer Charts   |
| in_shazam_charts      | Contains the track's occurrences in Shazam Charts   |
| bpm                   | Contains the track's beats per minute |
| key                   | Contains the key of a song|
| mode                  | Contains the mode of a song          |
| danceability_%        | Contains the danceability of a song      |
| valence_%             | Contains the valence of a song          |
| energy_%              | Contains how much energy a song has         |
| acousticness_%        | Contains how much acoustics a song has    |
| instrumentalness_%    | Contains the key of a song  |
| liveness_%            | Contains how much live recording a song has          |

---

### 3. Task Goals
- Practice cleaning data/ data preparation
- Analyze and visualize the cleaned data
- Explore relationships and gain insights from the different attributes of the dataset

---

### 4. Installation and Setup

- **Requirements**
  Install the following Python libraries:
  
     `pandas`: For data manipulation and analysis.
  
     `matplotlib`: For data visualization.
  
     `seaborn`: For statistical data visualization.
  
     `numpy`: For numerical operations.
  
- **Environment**:
  This notebook was developed using Python 3.x.
  
- **How to Run**:
  Open the notebook in Jupyter Notebook or Jupyter Lab. If you don’t have Jupyter installed, you can install it with: `pip install jupyter`
  After the notebook opens, execute each cell sequentially to run the analysis.

---

### 5. Data Exploration

#### Loading the Data
```python 
df_spotify = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
```
This line code loads the dataset and stores it into a data frame named "df_spotify". The read_csv is used to read csv files as a dataframe

#### Initial Observations
For initial observation, this line of code was used:
```python
df_spotify.info()
```
It gets the basic information of the dataset, including its size, attributes, and data type per attribute. The output looks like this:
![image](https://github.com/user-attachments/assets/423cc02b-2a56-4382-a179-8d0d1538fb64)

From here, we can see that:

   The dataset has 953 rows and 24 columns

   All the datatypes of each column is either a 64-bit integer or an object. It's obvious from the getgo that `streams`, `in_deezer_playlists`, and          `in_shazam_charts` are incorrectly detected as an object datatype instead of a float or an integer.

- Check for duplicated values

  To check for duplicated values, the block of code was used below:
  
  ```python
  duplicate_tracks = pd.DataFrame(df_spotify[df_spotify.duplicated(['track_name','artist(s)_name'])])
   # checks duplicated tracks

   print("The duplicate tracks that is in the dataset are ")
   print()
   print(duplicate_tracks[['track_name','artist(s)_name']])
  ```

   The code above stores the duplicates in the original dataset in a new data frame and outputs all the duplicated tracks shown below:

  ![image](https://github.com/user-attachments/assets/11f6c23e-2823-472c-aa33-e9c47b769fe3)

   The duplicated values that are only checked are the title of the song since the other statistics would vary due to source data anomalies. It also only lists the second instance of the duplicated values, it will be delved deeper into later which is the right instance to be kept.

- Check for missing values

  To check for missing values, the block of code was used below:

  ```python
  missing_values = df_spotify.isna().sum()

   print("The attributes that have missing values in the dataset are ")
   print()
   print(missing_values[missing_values>0])
  ```

  The code above stores the number of times there are missing values in each column of the dataset, and it outputs the image below:
  
  ![image](https://github.com/user-attachments/assets/6b58c84b-6930-4efc-9c3c-e1990b1d1d6b)

  Here, we can see that there are 50 missing values in the attribute of `in_shazam_charts`, and 94 in the `key` attribute. This is also due to source data anomalies, and dealing with it will be discussed later in the next part, which is data preparation/cleaning.
  
---

### 6. Data Cleaning/Preparation

#### Handling wrong datatypes

   - To fix the data type of each attribute, each one had a unique process that had to go through.

     To fix the data type of the streams attribute, the line of code was used below

     ``` python
     df_spotify['streams'] = pd.to_numeric(df_spotify['streams'], errors = 'coerce')
     ```

     The line of code above uses pd.to_numeric to turn all the values inside the inputted column into a numerical variable; the error = 'coerce' part was needed after an error popped up that it could not convert a single value indexed at 574 to be turned into a numerical value. It would seem that it contains the other track's information on the other attributes, as can be seen below.

     ``` python
     
     

     

#### Handling Duplicate Values
   - I handled the duplicated values by comparing the info of both instances side by side to check what has the correct data and to keep it, and to drop the one instance that has the wrong data.

   For the first song which is "SPIT IN MY FACE!" by ThxSoMch

  ``` python
df_spotify.loc[(df_spotify['track_name']=='SPIT IN MY FACE!')]
```

   Uses .loc to locate a specific value of a specific attribute of a dataset, the outputted side-by-side info looks like this

  ![image](https://github.com/user-attachments/assets/0b8f4be5-a150-4223-9a79-9a25f2f25499)

  ![image](https://github.com/user-attachments/assets/f4baaafa-c029-41b7-a77d-1652b8935d52)

  Upon inspection, their key and beats per minute do not match the other values. Upon research, the correct bpm for this song is 94, according to Gateway (2024), and it is also closer to the other musical attributes from the same source. Fortunately, it also includes the higher number of recorded song statistics between the two indicating that this is the latest data log of the track.

To drop the wrong instance, which is indexed at 482, the code is used below:

``` python
df_spotify.drop(index=482, inplace=True)
```

   This line of code drops the wrong instance, indexed at 482, and inplace=True makes it so that it drops the row in the original dataset as well.
   
   Repeating the same process for the rest of the duplicates, we have

   The 2nd song is "Take My Breath" by The Weeknd
   
   ``` python
df_spotify.loc[(df_spotify['track_name']=='Take My Breath')]
```
   Output

   ![image](https://github.com/user-attachments/assets/77ee430b-6075-46d3-9122-f1a8df445db3)

   ![image](https://github.com/user-attachments/assets/850830f4-c364-41f4-b134-30b5009a6c49)

   Upon further research, the correct key for the song is A flat major, according to Gateway (2024b), and its enharmonic equivalent is G sharp major, according to North Coast Synthesis Ltd. (2024). Fortunately, the correct instance also has a higher stream count, indicating that it is the most recent log of data for the track. We should, therefore, drop the instance indexed at 512.

   ``` python
df_spotify.drop(index=512, inplace=True)
df_spotify.loc[(df_spotify['track_name']=='Take My Breath')]
```

   The 3rd song is "About Damn Time" by Lizzo 

   ``` python
df_spotify.loc[(df_spotify['track_name']=='About Damn Time')]
```

Output

![image](https://github.com/user-attachments/assets/f30fa356-03d5-4417-a71d-e2f63bf37ee9)

![image](https://github.com/user-attachments/assets/9f217ea9-6fc5-4b91-b668-6bfb6d089261)

 Upon further research, the correct release date for the song is April 14th, 2022, according to Wikipedia (2024). Fortunately, the correct instance also has a higher stream count, indicating that it is the most recent log of data for the track. We should, therefore, drop the instance indexed at 372.

 ```python
df_spotify.drop(index=372, inplace=True)
```

   The 4th song is "SNAP" by Rosa Linn

   ``` python
df_spotify.loc[(df_spotify['track_name']=='SNAP')]
```

Output

![image](https://github.com/user-attachments/assets/7f90e1ba-010f-4376-9a9d-50e0b5ae474f)

![image](https://github.com/user-attachments/assets/db01afe4-615e-4e58-aff6-7e8488ff3d5d)

Since the two have a negligible difference, we should keep the instance with the greater count of streams. Therefore, we should drop the instance indexed at 873.

``` python
df_spotify.drop(index=873, inplace=True)
```

#### Handling Missing Values 
   #### For Missing Keys
   
   - I handled missing values in the key attribute by filling them with a placeholder. I did not drop them from the dataset as they aren't close to 50% missing of the entire population. By replacing the missing keys with a placeholder, valid data is retained, and it can be rejected for later consideration for data analysis related to the key attribute. An easily implemented imputation method cannot be applied here since it is a categorical value. The line of code below is used to implement this

     ``` python
     df_spotify['key'].fillna('Missing', inplace=True)
     ```

     The line of code uses .fillna, which fills up all the missing values with the given input, and inplace=True makes it so that the edit is done to the original dataset and not a copy.

     Checking the updated missing values using the code below;

     ``` python
     missing_values = df_spotify.isna().sum()
     print("The updated missing values are now ")
     print()
     print(missing_values[missing_values>0])
     ```

     Output

     ![image](https://github.com/user-attachments/assets/1d5f5996-f42f-4763-8cfe-740fcb7d6874)

     There are now only 50 missing values in the Shazam chart attribute. For these missing values, I implemented an imputation method as it was a numerical variable, and according to Chandrikasai (2023), an imputation method can be used when the missing values in a column are less than 10% of the whole population which in this case is true. The imputation method that I chose was to fill in the missing values with the median of the attribute. I chose this imputation method as it assumes that the missing data is missing completely at random, according to Keita (2023), and almost all data analysis algorithms/software assumes that data are missing completely at random, according to The Data Story Guide (n.d.). Nevertheless, it should be noted that all imputation has a con and might distort results according to Subha (2024), but since it is less than 10% of the population and the value used to replace is a valid imputation method, I think it will have little to no effect to distort the results.

     The code below is used to implement the imputation method. We first store the calculated median value in a variable, check what the median is, and then use it to fill in the missing values.

     ``` python
     shazam_median = df_spotify['in_shazam_charts'].median()
     f"The median value of the 'in_shazam_charts' is {shazam_median}"
     ```

     The first line of code uses .median(), which calculates the median of a given column and stores the value of the median in shazam_median
     The second line of code prints out the information on the value of the median
     
     Output
     ![image](https://github.com/user-attachments/assets/d79231ff-8210-4bd6-aab3-56d8c950306f)

     The next code is used to replace all the missing values with the median value.

     ```python
     df_spotify['in_shazam_charts'].fillna(shazam_median, inplace=True)
     ```

     The code above uses .fillna, which replaces all missing values with an inputted value, and inplace=True is used so it is edited in the original dataset

---

### 7. Analysis

#### Overview of Dataset
- The dataset originally had 953 rows, but after dealing with the duplicates, it became 949 rows, with the same amount of columns.

  The data types of the dataset were either a 64-bit integer or an object. Furthermore, inspecting the data types, it would seem that the attributes of streams, in_deezer_playlists, in_shazam_charts, were incorrectly detected as an object data type. Thankfully, this was dealt with during the data pre-processing/preparation.

  At first, after handling all the duplicate tracks, there were missing values in the streams column, in_shazam_charts, and key columns. The missing value in the streams column was replaced by a rough estimate.

  All the rows that have a missing key were dropped. Lastly, the rest of the missing values in the in_shazam_charts were replaced by the median of the column

#### Basic Descriptive Statistics
- The total count, mean, median, and standard deviation of the stream attribute are as follows:

  | Data              | Streams                       |
  |---------------------|--------------------------------------|
  | Total            | 487818415565         |
  | Mean       | 514034157.60273975      |
  | Median        | 288101651.0       |
  | Standard Deviation        |567574041.7934494   |

  It would seem that the tracks with the greater streams highly influence the average streams, and this is also corroborated with the high standard deviation. The median also suggests that half of the track has less than 300 million streams. Let us dig deeper and see the top 10 most streamed tracks to see what is their value and the distribution of the streams. The code used for that is seen below

  ``` python
  df_spotify[['track_name', 'streams']].sort_values(by='streams', ascending=False).head(10)
   # ascending=False sorts the tracks by greatest to least number of streams, gets the top ten by .head(10),
   # and only prints the title and its number of streams
   ```

   Output

  ![image](https://github.com/user-attachments/assets/7235b1fc-15e8-4efa-98ac-e63078b63a62)

   It would seem that the top 2 highest streamed tracks have over 3 billion streams, which heavily affects the calculation of the mean and standard deviation; other than that, the rest of the top 10 also have over 2 billion streams. Next would be the distribution of the streams to visualize just how many tracks, and this is done by implementing the code below.

  ``` python
  sns.histplot(df_spotify, x= 'streams',color='blue', element="poly").set(title = 'Distribution of total streams')
   # plots the distribution of the stream count, sets the colo and the title
   sns.set_style("whitegrid", {'grid.linestyle': '--'})S
   # designs the grid

  plt.xlabel('Number of Streams by Billions')
   plt.ylabel('Number of Tracks')
   # labels the x and y title
  ```

  Output

  ![image](https://github.com/user-attachments/assets/9da19703-10d7-4fcb-9fe2-3afd8c343c62)

  The plot further suggests that the tracks with the higher number of streams heavily influence the calculated mean and standard deviation of the streams. It can also be seen that over 200 tracks did not even crack the half-billion mark, yet it is the calculated mean.

  Now, let us check the distribution track's artist count and release year to see if it has any correlation to their number of streams.

  First, to plot for the distribution of the track's release year, the block of code was used below

  ```python
  ryd = sns.displot(df_spotify, x='released_year',color='blue', discrete=True, aspect=3).set(title = "Distribution of the most streamed spotify songs of 2023 by released year")
   # To plot the distribution of the released_year along with its distribution line, and each year is represented
   # also labels title

   ax = ryd.ax
   ax.grid(True, axis="y", color="gray", alpha=0.5)
   # to place grids along the x_axis

   plt.xlabel('Track Release Year')
   plt.ylabel('Number of Tracks')
   # labels x and y axes
  ```

  Output

  ![image](https://github.com/user-attachments/assets/02dfcb8a-6245-42ed-95ca-9279dd1f2db5)

  It would seem the most streamed Spotify songs of 2023 are mostly from the 2020s and largely from the year 2022. This makes sense as trendy and topical music gets played over and over as they are newly released
  
  Tracks from 2022 would also accumulate more plays as it had more time to earn it than the tracks that were released in the year 2023. It also represents the time it takes for a song to rise in popularity. It would also seem that the distribution of the released year of the song follows recency bias, except for some goldies and some retro songs of the 2000s.

  To track the outliers, we have to first calculate the interquartile range to find the outliers and count them all. For this, the block of code was used below

  ``` python
  Q1ry = df_spotify['released_year'].quantile(0.25) 
   # 1st quartile

   Q3ry = df_spotify['released_year'].quantile(0.75)
   # 2nd quartile
   IQRry = Q3ry - Q1ry

   # Equation for finding outliers 
   outliers_ry = df_spotify[(df_spotify['released_year'] < Q1ry - 1.5 * IQRry) | (df_spotify['released_year'] > Q3ry + 1.5 * IQRry)]

   ry_outliers_num = outliers_ry.shape[0]
   # gets the number of outliers

   f"There are about {ry_outliers_num} outliers within the release year attributes of the tracks."
  ```

  Output

  ![image](https://github.com/user-attachments/assets/77c99b01-85f4-435e-ab10-ca65f077fda0)

  To visualize the number of outliers, a boxplot can be used to do this. As such, the block of code was implemented below.

  ``` python
  plt.figure(figsize=(15, 5))
   # To size the boxplots

   sns.boxplot(x=df_spotify['released_year'], fliersize=3)
   # Creates boxplot and resize the outlier data points

   plt.title("Track Release Year Boxplot") 
   # Sets title for the boxplot
   plt.xlabel("Track Release Year")
   # Labels x-axis
   plt.show()
  ```

  Output

  ![image](https://github.com/user-attachments/assets/8f7f431f-29a9-4cd5-898e-6f2a02b0ee30)

   It would seem that there are about 151 outliers when it comes to the track's release year being on the list of the most streamed Spotify songs of 2023. What stands out from the plot is the one song from the 1940's and a lot of them are cooped up around the 2010s and early 2000s.

   For the artist count, the block of code was used below.

  ``` python

  ac = sns.displot(df_spotify, x='artist_count', color='Blue', discrete=True).set(title = "Distribution of the number of credited artists per track")
   # To plot the distribution of the released_year along with its distribution line, and each year is represented
   # To plot the distribution of the released_year along with its distribution line, and each artist count being represented
   # also labels title

   sns.set_style("white")

   ax = ac.ax
   ax.grid(True, axis="y", color="gray", alpha=0.5)
   # to place grids along the x_axis

   plt.xlabel('Number of Credited Artists')
   plt.ylabel('Number of Tracks')
   # labels x and y axes
  ```

   Output

   ![image](https://github.com/user-attachments/assets/c54154d4-53cd-421c-9e3c-9e23f7f709ba)

   
   It would seem that most of the songs in the dataset are credited to one artist. The more artists are credited to a song, the less likelihood it will make the list of the most streamed. To check for outliers, the same process used as before will be applied.

  ```python
  Q1ac = df_spotify['artist_count'].quantile(0.25) # 1st quarter 
   Q3ac = df_spotify['artist_count'].quantile(0.75) # 3rd quarter
   IQRac = Q3ac - Q1ac
   # Calculate interquartile

   # Equation for finding outliers within a given data
   outliers_ac = df_spotify[(df_spotify['artist_count'] < Q1ac - 1.5 * IQRac) | (df_spotify['artist_count'] > Q3ac + 1.5 * IQRac)]
   ac_outliers_num = outliers_ac.shape[0]
   f"There are about {ac_outliers_num} outliers within the release year attributes of the tracks."
  ```

  Output

  ![image](https://github.com/user-attachments/assets/3052a1d1-d8b4-40b7-96f6-0870b5830157)

  For visualization,

  ```python
  plt.figure(figsize=(15, 5))
   # To size the boxplots

   sns.boxplot(x=df_spotify['artist_count'], fliersize=3)
   # Creates boxplot and resize the outlier data points

   plt.title("Number of Credited Artist Per Track Boxplot") 
   # Sets title for the boxplot
   plt.xlabel("Track Release Year")
   # Labels x-axis
   plt.show()
  ```

  It would seem that if a track has 3 or less credited artists, the greater likelihood it will appear on the given dataset.

#### Top Performers

  The top 5 performing tracks by streams can be seen by using the code below

  ```python
   df_spotify[['track_name', 'artist(s)_name','streams']].sort_values(by='streams', ascending=False).head()
   # it sorts the data from greatest to least streams
   # it locates and print the first 5 tracks along with its track name and the number of streams
   ```

   Output

   ![image](https://github.com/user-attachments/assets/6581ab4d-3c22-48ce-bc3a-0c825b17a6d1)

   The top 5 most streamed tracks can be seen above. The most streamed track by far is the song 'Blinding Lights' by The Weeknd. Interestingly enough, one of the top 5 songs by streams has two artists credited on it.

   To the top 5 performing artists by track count, we need to separate all the artists into each of their own rows so that we can count their streams correctly.

   First, we should separate all the artists by using the code below

   ```python
   df_spotify = df_spotify.explode('artist(s)_name').reset_index(drop=True)
   df_spotify['artist(s)_name'] = df_spotify['artist(s)_name'].str.strip()
   # The .explode() makes it so that the list is counted as a row, we also reset index for cleaner analysis
   # The list of the separated names is now stored in the column of the artist(s)_name
   ```

   Since the artists are now separated, we should also drop the artist count column as it is no longer valid. The EDA part on the artist count is also already done so we should implement this now.

   ```python
   df_spotify.drop('artist_count',axis=1,inplace=True)
   # drops the artist_count attribute from the whole dataset
   df_spotify.rename(columns={'artist(s)_name':'artist_name'}, inplace=True)
   # renames the attribute of artist name properly
   ```

   Now we can finally properly see the top 5 artists by track count

   ```python
   df_spotify['artist_name'].value_counts().head()
   ```

   Output

   ![image](https://github.com/user-attachments/assets/9c39a24d-e0c2-4cba-8e06-7b9323d29059)

   It would seem that Bad Bunny is number one in terms of the number of tracks appearing in the dataset. Taylor Swift being number two makes sense as she re-releases some of her tracks with different versions as well. Kendrick Lamar being top 4 makes sense, as his newest album was released in 2022.

#### Temporal Trends

   To check the temporal trends, we can start by checking the number of tracks released over time

   For tracks released over the year, it can be checked using the code below

   ```python
   sns.displot(data=df_spotify, x='released_year', element='poly', fill=False, discrete=True, aspect = 3)
   # plots the tracks released vs the year it was released

   plt.xlabel('Months')
   plt.ylabel('Number of Tracks Released')
   plt.title('Number of Tracks Released Per Month')
   # labels the different parts of the plot such as the x-axis, y-axis, and the title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/19d18bbd-4b4c-4607-9ead-365d30bc016b)

   It shows the same trend that most of the songs in the dataset were released in the 2020, as observed before. It also favors recency bias as the newer tracks gain more streams daily.
   
   For the release over the months, the code below can be used

   ```python
   sns.displot(data=df_spotify, x='released_month', element='poly', fill=False, discrete=True, aspect = 3)
   # plots the tracks released vs the months it was released on
   sns.set_style("whitegrid", {'grid.linestyle': '--'})

   plt.xlabel('Months')
   plt.ylabel('Number of Tracks Released')
   plt.title('Number of Tracks Released Per Month')
   # labels the different parts of the plot such as the x-axis, y-axis, and the title


   plt.xticks(ticks=range(1, 13), labels=[calendar.month_name[i] for i in range(1, 13)])
   # uses calendar library to name the months for the x-axis

   plt.xlim(1, 12)
   # shows all the months
   plt.show()
   ```

   Output

   ![image](https://github.com/user-attachments/assets/d4b284ed-3b6f-4785-a852-1898a360965d)

   The months that saw the most tracks are both January and May. It would seem that the track release over the months does not follow any patterns or biases. To further explore this, we should track the release trend over the months from the year 2019-2023, as that is the years where most of the tracks are from in the dataset. To implement this the block of code is used below

   ```python
   
   tracks_years_data = pd.DataFrame()
   # Create a seperate dataframe for selected years

   for year in range(2019, 2024):
       df_sub = df_spotify[df_spotify.released_year == year].groupby('released_month').size().reset_index(name='track_count')
       df_sub['year'] = year  # Add the year column
       tracks_years_data = pd.concat([tracks_years_data, df_sub], ignore_index=True)  # Concatenate to the seperate dataframe

   # For loop for each year 2019-2020 to get the release month of all the tracks

   plt.figure(figsize=(15, 5))
   # Sets size of the plot

   sns.lineplot(x='released_month', y='track_count', hue='year', data=tracks_years_data)
   sns.set_style("whitegrid", {'grid.linestyle': '--'}) 
   # Uses seaborn to plot the yearly data vs the track release month, and designs the grid

   plt.title("Monthly Track Releases by Year")
   plt.xlabel("Month")
   plt.ylabel("Number of Tracks")
   # Labels the plots title, x-axis, and y-axis

   plt.xticks(ticks=range(1, 13), labels=[calendar.month_name[i] for i in range(1, 13)])
   # x-labels as months using the calendar library

   plt.legend(title="Year")
   # Shows the legend for each line

   plt.show()
   ```

   Output

   ![image](https://github.com/user-attachments/assets/261a50e5-63a5-4c94-a238-ed6d41cd3671)

   It would also still seem as if the track release over the months has no pattern and does not follow any trends or biases.

#### Genre and Music Characteristics

   To find if there is any correlation between the musical characteristics of a song and its number of streams. A heatmap is implemented to visualize if there is any positive or negative correlation between them. The block of code below can be used below to implement this

   ```python
   musical_attr = ['streams', 'bpm','danceability_%','valence_%','energy_%','acousticness_%','instrumentalness_%','liveness_%','speechiness_%']
   # Set up seperate data to only include streams and the track's musical attributes

   df_musical_attr = df_spotify[musical_attr]
   # Turn the seperate data into a dataframe

   corr_music_attr = df_musical_attr.corr()
   # calculates correlation between all the streams and musical attributes

   sns.heatmap(corr_music_attr, annot=True, vmin = -1, vmax = 1, cmap='BuPu')
   # creates heatmap to visualize all the correlations

   plt.title("Heatmap of Streams & Different Music Attributes")
   ```

   Output

   ![image](https://github.com/user-attachments/assets/83041b19-6480-4e47-bd3b-75a830280af5)

   It would seem that all of the musical attributes have a very weak negative correlation as they all have values from -0.01 to -0.19
   
   The musical attribute that has the least correlation to the number of streams is the bpm or beats per minute
   
   The strongest weak negative correlation is both danceability and speechiness being both -0.11 respectively. These two seem to influence the number of streams a song has, although negative and very weak.
   
   The danceability and energy attributes has a very weak positive correlation, it is exactly at 0.18
   
   The valence and acousticness attributes has also a very weak positive correlation, it is exactly at -0.029
   
   The strongest weak positive correlation is between valence and danceability, which is at 0.41
   
   The second strongest weak positive correlation is between valence and energy, which is at 0.36

   

#### Platform Popularity

   To check if there are patterns for a track's popularity among different platforms. We should compare the attributes of the different platforms, such as their playlists and charts. Let use both check the total and mean occurences to see if there is a pattern or any bias. To implement this, the block of code below is used

   For the total occurrences in playlists across different platforms

   ```python
   platform_playlists = ['in_spotify_playlists','in_deezer_playlists','in_apple_playlists']
   # Select the specified platform statistics

   sns.barplot(data=df_spotify[platform_playlists].sum(), palette='viridis')
   # Creates barplot with their respecive means and also colors it

   plt.yscale('log')
   # Scales the bar sizes

   plt.title("Platform Playlist Occurrences vs. Number of Occuring Tracks Barplot")
   plt.xlabel("Platform Playlist Occurreneces")
   plt.ylabel("Total Number of Occurrences")
   # Labels the plot's title, x-axis, and y-axis
   ```

   Output

   ![image](https://github.com/user-attachments/assets/cbb6c190-0a09-4026-8a64-44e16050543f)

   
   For the mean occurrences in playlists across all platforms

   ```python
   platform_playlists = ['in_spotify_playlists','in_deezer_playlists','in_apple_playlists']
   # Select the specified platform statistics

   sns.barplot(data=df_spotify[track_platform_selected].mean(), palette='viridis')
   # Creates barplot with their respective means and also colors it

   plt.yscale('log')
   # Scales the bar sizes

   plt.title("Platform Playlists Mean vs. Mean of Number of Occuring Tracks")
   plt.xlabel("Selected Platform Statistics")
   plt.ylabel("Mean Number of Occurrences")
   # Labels the plot's title, x-axis, and y-axis
   ```

   Output

   ![image](https://github.com/user-attachments/assets/e312b205-b4b5-4e2f-beb2-bbe4ef515df5)

   ```python
   platform_playlists = ['in_spotify_playlists','in_deezer_playlists','in_apple_playlists']
   # Select the specified platform statistics

   palette_color = sns.color_palette('pastel') 
   # colors the pie chart

   plt.figure(figsize=(12,12))
   # resizes the pie chart

   plt.pie(df_spotify[platform_playlists].sum(), labels=platform_playlists, colors=palette_color, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Playlist Counts Pie Chart")
   # sets the title of the pie chart
   ```

   Output

   ![image](https://github.com/user-attachments/assets/605e0509-f551-42f9-bf9d-20a8a0f2736e)

   For the pie chart visualization of the division of the total streams

   ```python
   platform_playlists = ['in_spotify_playlists','in_deezer_playlists','in_apple_playlists']
   # Select the specified platform statistics

   palette_color = sns.color_palette('pastel') 
   # colors the pie chart

   plt.figure(figsize=(12,12))
   # resizes the pie chart

   plt.pie(df_spotify[platform_playlists].sum(), labels=platform_playlists, colors=palette_color, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Playlist Counts Pie Chart")
   # sets the title of the pie chart
   ```

   Output

   ![image](https://github.com/user-attachments/assets/724a1fc1-effb-4cc5-85de-f0182ba0d348)

   It would seem that the more the track is in Spotify playlists, the more it is streamed. The Apple playlist does not seem to be much of an indicator of how much a track is streamed.
   
   Spotify playlists being the biggest indicator of the most streamed tracks makes sense as it is the more accessible, and customizable app for the masses. It is also worth noting that 92% of the total playlist occurrence comes from Spotify Playlists

   Both Spotify and Deezer (in specific countries) are not locked by any paywall and are free to use, unlike Apple Music.

   Let us dive deeper by comparing the different platform's statistics directly by their playlist counts and chart counts, by using the same process

   For the total count of playlist occurrences barplot

   ```python
   platform_charts = ['in_spotify_charts','in_apple_charts','in_deezer_charts','in_shazam_charts']
   # Seperate the chart attributes

   sns.barplot(df_spotify[platform_charts].sum(), palette='viridis')
   # Creates barplot with the means of all platform charts and also colors it

   plt.title("Total Platform Charts vs. Number of Tracks Barplot")
   plt.xlabel("Platform Charts")
   plt.ylabel("Number of Tracks")
   # Labels the plot's title, x-axis, and y-axis
   ```

   Output

   ![image](https://github.com/user-attachments/assets/57023c45-5299-4578-984c-7d97e67551d2)

   For the mean of playlists occurrences barplot

   ```python
   platform_charts = ['in_spotify_charts','in_apple_charts','in_deezer_charts','in_shazam_charts']
   # Seperate the chart attributes

   sns.barplot(df_spotify[platform_charts].mean(), palette='viridis')
   # Creates barplot with the means of all platform charts and also colors it

   plt.title("Mean of Platform Charts Barplot")
   plt.xlabel("Platform Charts")
   plt.ylabel("Mean Number of Occurrences")
   # Labels the plot's title, x-axis, and y-axis
   ```

   Output

   ![image](https://github.com/user-attachments/assets/a2e3c178-7292-438d-8ef6-3d75eb5f259b)

   For the total count of playlist occurrences pie chart

   ```python
   platform_charts = ['in_spotify_charts','in_apple_charts','in_deezer_charts','in_shazam_charts']

   palette_color = sns.color_palette('pastel') 
   # colors the pie chart

   plt.figure(figsize=(12,12))
   # resizes the pie chart

   plt.pie(df_spotify[platform_charts].sum(), labels=platform_charts, colors=palette_color, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Chart Counts Pie Chart")
   # sets the title of the pie chart
   ```

   Output

   ![image](https://github.com/user-attachments/assets/e9250f52-7cc2-4d05-b761-2df22504c932)

   It would seem in terms of charts across all platforms. The charts of Apple Music and Shazam seem to be the biggest indicator of the tracks being in the list of the most streamed Spotify songs of 2023. Oddly enough, this time Deezer and Spotify charts are not a big indicator in this facet. 

   It is also worth noting that Apple Music and Deezer Charts account for 42% and 46%, respectively, of the total charts.

#### Advanced Analysis

 To perform advanced analysis, we were tasked to find the number of tracks per key in order to see if there is a trend or a pattern. Let us first group the number of tracks by their labeled keys, and then plot them into a bar plot and a pie chart for visualization and analysis.

 We should also study the performance of the artists across all platform statistics.

 ```python
   df_key_counts = pd.DataFrame(df_spotify.groupby("key").size())
   # groups track by their key

   df_key_counts = df_key_counts.sort_values(by='key').reset_index()
   # sorts the key alphabetically and resets the index

   df_key_counts = df_key_counts.rename(columns={0: 'number_of_tracks'})
   # renames second column as number of tracks

   df_key_counts = df_key_counts[df_key_counts['key'] != 'Missing']
   # Removes missing counts 

   df_key_counts
   ```

   Output

   ![image](https://github.com/user-attachments/assets/e9d894fa-49ee-4701-a266-90b5d56b33c7)

   For the barplot of key of tracks

   ```python
   sns.barplot(df_key_counts, x = 'key', y = 'number_of_tracks', palette='Set2')
   # Creates barplot of key counts and colors it


   plt.title("Key Count Barplot")
   plt.xlabel("Keys")
   plt.ylabel("Number of Tracks")
   # Labels the plot's title, x-title, y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/f2a666dd-1cc3-4241-bce3-4bd4f2dd5a83)

   For the pie chart of key of tracks

   ```python
   number_of_tracks = df_key_counts['number_of_tracks']
   # stores tack count in a seperate variable

   keys = df_key_counts['key']
   # stores keys in a seperate variable


   colors = ['#ff9999', '#66b3ff', '#99ff99', '#ffcc99', '#c2c2f0', '#ffb3e6', '#c4e17f', '#76d7c4', '#f7b7a3', '#d4a5a5', '#a3c4f7']

   plt.figure(figsize=(14,14))
   # resizes the pie chart

   plt.pie(number_of_tracks, labels=keys, colors=colors, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Key Counts Pie Chart")
   # sets the title of the pie chart
   ```

   Output

   ![image](https://github.com/user-attachments/assets/ad75ebaf-b301-46ec-97fd-95bf1d5ed37b)

   It would seem that the C sharp key has the most tracks, at exactly 119 tracks and being 14% of the whole population. The runner-up would be G and G sharp at 96 and 91 tracks respectively and both having 11% of the whole population.

   For comparing the keys vs. streams, we can both have the total stream per key and the mean stream per key. As well as a pie chart to visualize and analyze if there is any correlation between the two.

   For the bar chart of the total count of streams per key

   ```python
   sns.barplot(df_key_streams, x = 'key', y = 'streams', palette='Set2')
   # Creates barplot with keys vs. total streams

   plt.title("Keys vs. Total Streams Barplot")
   plt.xlabel("Keys")
   plt.ylabel("Streams by One Hundred Billion")
   # Labels title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/e893982b-9948-4098-a723-82d07e36bf5c)

   For the bar chart of the mean stream per key

   ```python
   df_key_streams['mean_streams'] = df_key_streams['streams']/df_key_counts['number_of_tracks']
   # adds the mean of streams to the same dataframe

   sns.barplot(df_key_streams, x = 'key', y = 'mean_streams', palette = 'Set2')

   plt.title("Keys vs. Stream Mean Barplot")
   plt.xlabel("Keys")
   plt.ylabel("Stream Mean by Hundred Millions")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/ec8a2e54-0911-4f13-ae4a-5b093190e78f)

   For the pie chart of the total streams per key 

   ```python
   number_of_streams = df_key_streams['streams']
   # stores the number of streams in a seperate variable

   keys = df_key_counts['key']
   # stores the keys in a seperate variable

   colors = ['#ff9999', '#66b3ff', '#99ff99', '#ffcc99', '#c2c2f0', '#ffb3e6', '#c4e17f', '#76d7c4', '#f7b7a3', '#d4a5a5', '#a3c4f7']

   plt.figure(figsize=(14,14))
   # resizes the pie chart

   plt.pie(number_of_streams, labels=keys, colors=colors, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Key Streams Pie Chart")
   # sets the title of the pie chart
   ```

   Output

   ![image](https://github.com/user-attachments/assets/2855bf3e-b235-4662-a41c-90493b155dd3)

   It would seem that the C sharp is top one of the total stream count and accounts for 16% of the number of total streams. This may also due to the fact that it also has the most tracks between the keys.

   When comparing their mean of streams, the top one becomes the D key followed by the E key and C sharp. Which shows that even if the D key has less tracks, on average, its being listened to more than the others.

   For the mode analysis, we should group the tracks by mode and then order it by streams. Then we should compare their total track count, total stream count, and stream mean. All of these is plotted in a bar plot with the total counts also having pie charts. The following are implemented below with their corresponding output

   For the grouping the tracks by mode

   ```python
   df_mode_counts = pd.DataFrame(df_spotify.groupby("mode").size())
   # groups track by their mode

   df_mode_counts = df_mode_counts.sort_values(by='mode').reset_index()
   # sorts the key alphabetically and resets the index

   df_mode_counts = df_mode_counts.rename(columns={0: 'number_of_tracks'})
   # renames second column as number of tracks

   df_mode_counts
   ```

   Output

   ![image](https://github.com/user-attachments/assets/6162a615-5cee-46f7-8d67-ccb413e2dd5b)

   For the barplot of the total number of tracks per mode

   ```python
   sns.barplot(df_mode_counts, x = 'mode', y = 'number_of_tracks', palette='Set2')
   # Creates mode vs. number of tracks barplot and colors it

   plt.title("Mode vs. Total Tracks Barplot")
   plt.xlabel("Mode")
   plt.ylabel("Number of Tracks")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/42cc6a78-2407-44b0-8874-a4ef74c9ab21)

   For the piechart of the total track count per mode

   ```python
   number_of_tracks_mode = df_mode_counts['number_of_tracks']
   # stores the number of tracks in a seperate variable

   modes = df_mode_counts['mode']
   # stores the keys in a seperate variable

   colors = ['#ff9999', '#66b3ff']

   plt.figure(figsize=(6,6))
   # resizes the pie chart

   plt.pie(number_of_tracks_mode, labels=modes, colors=colors, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Mode Tracks Pie Chart")
   ```

   Output

   ![image](https://github.com/user-attachments/assets/d9a168f0-d367-44d6-b1e2-5c1a1c2600da)

   The major mode seems to have 548 tracks and accounts for more than half of the total tracks. 

   For analyzing the relationship between the mode and streams, we should first group them by mode and streams.

   ```python
   df_mode_stream = df_spotify[['mode', 'streams']].groupby('mode').sum('streams')
   # creates a seperate dataframe where in the total streams are grouped by mode

   df_mode_stream = df_mode_stream.sort_values(by='streams', ascending=False).reset_index()
   # sorts the stream value by greatest to least

   df_mode_stream
   ```

   Output

   ![image](https://github.com/user-attachments/assets/7936bf4b-b400-4ea5-a32b-1fd701ae6d85)

   Next step is plotting the barplot of total streams per mode

   ```python
   sns.barplot(df_mode_stream, x = 'mode', y = 'streams', palette = 'Set2')

   plt.title("Mode vs. Total Streams Barplot")
   plt.xlabel("Mode")
   plt.ylabel("Streams by Hundred Billions")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/60aa949d-3f0f-4bef-82cb-2c9f1e1f361d)

   Now for the piechart to see the division of the population

   ```python
   number_of_streams_mode = df_mode_stream['streams']
   # stores the number of tracks in a seperate variable

   modes = df_mode_counts['mode']
   # stores the keys in a seperate variable

   colors = ['#ff9999', '#66b3ff']

   plt.figure(figsize=(6,6))
   # resizes the pie chart

   plt.pie(number_of_streams_mode, labels=modes, colors=colors, autopct='%.0f%%') 
   # creates the pie chart and labels each slice

   plt.title("Mode Streams Pie Chart")
   ```

   Output

   ![image](https://github.com/user-attachments/assets/7a9df076-c84f-4e66-9698-6de07b19c851)

   It would seem that people listen to songs of the Major mode than the minor. This dominance may also be due to the fact that there are more songs in major than in minor in the whole dataset. Let us dig deeper and see which mode is more listened to in average. We should first calculate the mean and then plot it to visualize their difference.

   ```python
   df_mode_stream['mean_streams'] = df_mode_stream.streams / df_mode_counts.number_of_tracks

   sns.barplot(df_mode_stream, x = 'mode', y = 'mean_streams', palette = 'Set2')
   # Creates mode vs. mean of streams barplot and colors it

   plt.title("Mode vs. Stream Mean Barplot")
   plt.xlabel("Mode")
   plt.ylabel("Stream Mean by Hundred Millions")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/2da7d964-8d06-4ce5-adf9-149277ba63d8)

   It would seem that people still listen to tracks that have the major mode on average, but the difference between the two did get smaller.

   For the next part, we should analyze the performance of the artists across the playlists and charts across all the platforms.

   First, we should group the track by artists and playlist counts, then graph both the top 5 across each playlist count, and then the total playlist count. To implement this, the lines of code were used below with their corresponding output.

   ```python
   platform_playlists = ['in_spotify_playlists','in_apple_playlists','in_deezer_playlists']
   # groups all playlists attribute

   df_artist_playlists = df_spotify.groupby('artist_name').sum(platform_playlists)
   # groups by artists and adds all their occurrences in the different platform playlists

   df_artist_playlists = df_artist_playlists.sort_values(platform_playlists, ascending=False).reset_index()
   # orders occurrences in the diff platform playlists from greatest to least
   ```

   The code above groups and sorts the artists by their playlist counts. The next part plots the top 5 in each playlist count per artist.

   ```python
   fig, axes = plt.subplots(ncols=3, figsize=(20, 6))
   # Sets to plot 3 graphs at once

   sns.barplot(df_artist_playlists.head().sort_values('in_spotify_playlists'), x = 'artist_name', y = 'in_spotify_playlists', palette='Set2', ax=axes[0])
   # also orders from greatest to least and only plots the top 5 artists
   # also sets the title for the plot

   axes[0].set(title='Artist vs. Occurance in Spotify Playlists Barplot')
   axes[0].set_xlabel('Top Occuring Artists')
   axes[0].set_ylabel("Number of Occurance in Spotify Playlists")
   # Labels the plot's title, x-title, and y-title

   sns.barplot(df_artist_playlists.head().sort_values('in_apple_playlists'), x = 'artist_name', y = 'in_apple_playlists', palette='Set2',             ax=axes[1]).set(title='Artist vs. Occurance in Apple Music Playlists Barplot')
   # plots the artist count vs. occurence in Apple Music playlists barplot and colors it
   # also orders from greatest to least and only plots the top 5 artists
   # also sets the title for the plot

   axes[1].set(title='Artist vs. Occurance in Apple Music Playlists Barplot')
   axes[1].set_xlabel('Top Occuring Artists')
   axes[1].set_ylabel("Number of Occurance in Apple Music Playlists")
   # Labels the plot's title, x-title, and y-title

   sns.barplot(df_artist_playlists.head().sort_values('in_deezer_playlists'), x = 'artist_name', y = 'in_deezer_playlists', palette='Set2',          ax=axes[2]).set(title='Artist vs. Occurance in Deezer Playlists Barplot')
   # plots the artist count vs. occurrence in deezer playlists barplot and colors it
   # also orders from greatest to least and only plots the top 5 artists
   # also sets the title for the plot

   axes[2].set(title='Artist vs. Occurance in Deezer Playlists Barplot')
   axes[2].set_xlabel('Top Occuring Artists')
   axes[2].set_ylabel("Number of Occurance in Deezer Playlists")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/762e43d4-375b-40b0-8384-a0efdd09ced3)

   The most consistent artists in the different platform playlist are:
   
   The Weeknd: Top 1 in Spotify, Top 1 in Apple Music, and Top 3 in Deezer
   
   Eminem: Top 2 in Spotify, Top 5 in Apple Music, and Top 1 in Deezer

   For the total playlist count across all platforms per artist

   ```python
   df_artist_playlists['total_playlists'] = df_artist_playlists['in_spotify_playlists'] + df_artist_playlists['in_apple_playlists'] +       df_artist_playlists['in_deezer_playlists']
   # calculates and stores total playlist count

   df_total_playlists = df_artist_playlists[['artist_name','total_playlists']].sort_values('total_playlists', ascending=False)
   # sorts from greatest to least total playlist count

   df_total_playlists.reset_index(drop=True, inplace=True)
   # resets index
   ```

   For plotting the bar plot for the total playlists count per artist

   ```python

   plt.figure(figsize=(14, 8))
   # sizes the plot

   sns.barplot(df_total_playlists.head(10), x = 'artist_name', y = 'total_playlists', palette ='Set2')
   # creates artists vs  total playlists barplot

   plt.title("Artists vs. Total Playlists Across All Platforms Occurrence Barplot")
   plt.xlabel("Artists")
   plt.ylabel("Total Playlist Count")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/93e42ff8-fd23-4aa4-8f33-0f9ce35c7a1d)

   It would seem that the top 5 occuring artists in each platforms playlist count is also the same top 5 occuring artists in total playlist counts

   For the next part, we should now check the performance of the artists of the charts of each platform and also across all platforms. The same process for the artist's playlist statistics is repeated for this one

   First, we should group the artists based on their playlist counts from greatest to least across different platforms, and then graph them in a barplot

   ```python
   platform_charts = ['in_spotify_charts','in_apple_charts','in_deezer_charts','in_shazam_charts']

   df_artist_charts = df_spotify.groupby('artist_name').sum(platform_charts)
   # groups by artists and adds all their occurances in the different platform pllaylists

   df_artist_charts = df_artist_charts.sort_values(platform_charts, ascending=False).reset_index()
   # orders occurances in the diff platform playlists from greatest to least
   ```

   Output

   ![image](https://github.com/user-attachments/assets/6de902c3-26f6-4cef-8e48-0270a1872643)

   The top artists that are consistently in charts of different platforms are:
   
   Bad Bunny: Top 1 in Spotify, Top 2 in Apple Music, Top 1 in Deezer, and Top 4 in Shazam
   
   The Weeknd: Top 2 in Spotify, Top 1 in Apple Music, Top 4 in Deezer, and Top 1 in Shazam
   
   Taylor Swift: Top 3 in Spotify, Top 3 in Apple Music, Top 5 in Deezer, and Top 2 in Shazam

   For the total chart counts per artist, the code was used below

   ```python
   df_artist_charts['total_charts'] = df_artist_charts['in_spotify_charts'] + df_artist_charts['in_apple_charts'] + df_artist_charts['in_deezer_charts'] +    df_artist_charts['in_shazam_charts']
   # calculates and stores total chart counts

   df_total_charts = df_artist_charts[['artist_name','total_charts']].sort_values('total_charts', ascending=False)
   # sorts greatest to least total chart count

   df_total_charts.reset_index(drop=True, inplace=True)
   # resets the index
   ```

   To graph the artist count and their total chart counts, the code was used below with it's corresponding output

   ```python
   plt.figure(figsize=(14, 8))
   # sizes the plot

   sns.barplot(df_total_charts.head(10), x = 'artist_name', y = 'total_charts', palette ='Set2')
   # plots artists vs total chart count barplot

   plt.title("Artists vs. Total Chart Occurrence Across All Platforms Barplot")
   plt.xlabel("Artists")
   plt.ylabel("Total Chart Count")
   # Labels the plot's title, x-title, and y-title
   ```

   Output

   ![image](https://github.com/user-attachments/assets/cede6479-637a-4fb5-ac63-e8ac2922c460)

   The Weeknd tops out both the total playlists and charts count, and with him that appears in both top 5 of the two categories are
Bad Bunny, Taylor Swift, and Peso Pluma.

   The occurrences of artists in playlists are more consistent than their occurrences in charts. Feid, who appeared in all of the top 5 chart counts across different platforms yet did not make the top 5 of the total char counts.

   Meanwhile, all of the top 5 artists in chart counts across different platforms were also the same artists in the top 5 among total chart counts.
   
---

### 8. Key Takeaways

We have found out that if the song has 3 or fewer credited artists, was released during the 2020s, is in tune with C sharp, and is in the major mode, the more likely that the song will show up on the given dataset. 

In terms of platform statistics, if the song is in Spotify playlists, Apple Music charts, and Shazam charts, it is more likely that the song will show up on the given dataset. It was also found that the artist frequency in playlists across all platforms is more consistent than the artist frequency in charts across all platforms.

In terms of the key of each song, songs with the key C sharp, D, and E, are the top-performing songs in terms of streams on Spotify in 2023.

Lastly, most tracks favor the major mode over the minor for the count of total tracks, as well as total streams, the stream median.

---

### 10. Key Errors and Learnings

The first significant error I encountered was when I tried to load the CSV file; the error code I received was UnicodeDecodeError. Upon researching, the way to fix this that worked was by adding encoding='ISO-8859-1' in the argument when loading in the file as pandas data frame. I got the idea from Saturn Cloud (2023). 

The second significant error was trying to convert the data type of the streams to a numerical data type. Upon researching, I fixed this by adding errors =' coerce'. I got the idea of converting the columns to numerical data types by fixing the error from GeeksforGeeks (2024).

The third significant error/problem I faced was editing the original dataset. I got the idea of putting 'inplace=True' in commands such as filling in missing values and dropping the wrong instance of the duplicates from GeeksforGeeks (2022).

For separating the artists to have their own row, specifically the command of .explode(), I learned it from pandas (n.d.).

For formatting and coding the different plots in Seaborn, I learned it from Michael Waskom, references are seen below.

I learned it from Bobbitt (2021) and Ghosh (2023) for labeling and sizing the different plots.

I specifically formatted and coded the pie chart in Seaborn; I learned it from GeeksforGeeks (2022a).

---

### References

Bobbitt, Z. (2021, August 5). How to add a title to Seaborn Plots (With examples). Statology. https://www.statology.org/seaborn-title/ 

Chandrikasai. (2023, April 10). Understanding the pros and cons of dropping missing values in data analysis. Medium. https://medium.com/@chandrikasai9997/understanding-the-pros-and-cons-of-dropping-missing-values-in-data-analysis-1bd8d2300fbe 

Elgiriyewithana, N. E. (2023). Most streamed Spotify Songs 2023. Kaggle. https://www.kaggle.com/dsv/6367938 

Gateway, M. G. (2024a). SPIT IN MY FACE! Key & BPM | ThxSoMch | Music Gateway. Music Gateway. https://www.musicgateway.com/song-key-bpm/thxsomch/spit-in-my-face 

Gateway, M. G. (2024b). Take My Breath - Single version Key & BPM | The Weeknd | Music Gateway. Music Gateway. https://www.musicgateway.com/song-key-bpm/the-weeknd/take-my-breath-single-version 

GeeksforGeeks. (2022a, February 28). How to create a pie chart in Seaborn? GeeksforGeeks. https://www.geeksforgeeks.org/how-to-create-a-pie-chart-in-seaborn/ 

GeeksforGeeks. (2022b, June 1). What does inplace mean in Pandas? GeeksforGeeks. https://www.geeksforgeeks.org/what-does-inplace-mean-in-pandas/ 

GeeksforGeeks. (2024, July 11). Convert pandas dataframe column to float. GeeksforGeeks. https://www.geeksforgeeks.org/convert-pandas-dataframe-column-to-float/ 

Ghosh, T. K. (2023, July 17). How to set axes labels & limits in a Seaborn plot? Tutorials Point. https://www.tutorialspoint.com/how-to-set-axes-labels-amp-limits-in-a-seaborn-plot 

Keita, Z. (2023, January 31). Top Techniques to Handle Missing Values Every Data Scientist Should Know. https://www.datacamp.com/tutorial/techniques-to-handle-missing-data-values 

kworb. (2024). Edison Lighthouse - Spotify Top Songs. https://kworb.net/spotify/artist/1NRzxuPpdGushT8YmF5NAa_songs.html 

North Coast Synthesis Ltd. (2024). key of A flat major - Chord Database. https://files.northcoastsynthesis.com/chords/key/AbM#:~:text=Enharmonic%20equivalent%3A%20G%E2%99%AFM. 

pandas. (n.d.). pandas.DataFrame.explode — pandas 2.2.3 documentation. https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.explode.html 

Saturn Cloud. (2023, October 22). How to Fix the Pandas UnicodeDecodeError utf8 codec cant decode bytes in position 01 invalid continuation byte Error | Saturn Cloud Blog. https://saturncloud.io/blog/how-to-fix-the-pandas-unicodedecodeerror-utf8-codec-cant-decode-bytes-in-position-01-invalid-continuation-byte-error/ 

Subha. (2024, February 21). Handling missing values in dataset — 9 methods that you need to know. Medium. https://medium.com/@pingsubhak/handling-missing-values-in-dataset-7-methods-that-you-need-to-know-5067d4e32b62 

The Data Story Guide. (n.d.). Checking and Understanding Missing Data. https://the.datastory.guide/hc/en-us/articles/4570444289167-Checking-and-Understanding-Missing-Data 

Waskom, M. (n.d.-a). seaborn.displot — seaborn 0.13.2 documentation. https://seaborn.pydata.org/generated/seaborn.displot.html#seaborn.displot 

Waskom, M. (n.d.-b). seaborn.heatmap — seaborn 0.13.2 documentation. https://seaborn.pydata.org/generated/seaborn.heatmap.html 

Waskom, M. (n.d.-c). seaborn.histplot — seaborn 0.13.2 documentation. https://seaborn.pydata.org/generated/seaborn.histplot.html 

Wikipedia. (2024, October 13). About Damn Time. https://en.wikipedia.org/wiki/About_Damn_Time

### Timeline of Activities


