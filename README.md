
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
  
This dataset contains a comprehensive list of the most famous songs of 2023 as listed on Spotify. The dataset offers a wealth of features beyond what is typically available in similar datasets. It provides insights into each song's attributes, popularity, and presence on various music platforms (Elgiriyewithana, 2023).

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

   - For fixing the data type of each attribute, each one had a unique process that it had to go through.

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

     The first line of code uses .median() which calculates the median of a given column and stores the value of the median in shazam_median
     The second line of code prints out the information of what is the value of the median
     
     Output
     ![image](https://github.com/user-attachments/assets/d79231ff-8210-4bd6-aab3-56d8c950306f)

     The next code is used to replace all the missing values with the median value.

     ```python
     df_spotify['in_shazam_charts'].fillna(shazam_median, inplace=True)
     ```

     The code above uses .fillna which replaces all missing values with an inputted value and inplace=True is used so it is edited in the original dataset

---

### 7. Analysis

#### Overview of Dataset
- The dataset originally had 953 rows, but after dealing with the duplicates, it became 949 rows, with same amount of columns.

  The data types of the dataset were either a 64-bit integer or an object. Furthermore, inspecting the data types, it would seem that the attributes of streams, in_deezer_playlists, in_shazam_charts, were incorrectly detected as an object data type. Thankfully this was dealt with inpart of the data pre-processing/preparation.

  At first, after handling all the duplicate tracks, there were missing values in the streams column, in_shazam_charts, key columns. The missing value in the streams column got replaced by a rough estimate.

  All the rows that has a missing key were dropped. Lastly, the rest of missing values in the in_shazam_charts got replaced by the median of the column

#### Basic Descriptive Statistics
- The total count, mean, meadian, and standard deviation of the streams attribute are as follows:

  | Data              | Streams                       |
  |---------------------|--------------------------------------|
  | Total            | 487818415565         |
  | Mean       | 514034157.60273975      |
  | Median        | 288101651.0       |
  | Standard Deviation        |567574041.7934494   |

  

#### Top Performers
- Identify the tracks and artists with the highest streaming counts.

#### Temporal Trends
- Explore trends in track releases over time and identify any notable patterns.

#### Genre and Music Characteristics
- Analyze relationships between genres, musical attributes, and streaming counts.

#### Platform Popularity
- Compare the popularity of tracks across different platforms (e.g., Spotify, Apple Music).

#### Advanced Analysis
- Perform any additional analysis, such as correlations or pattern analysis across keys or modes.

---

### 8. Key Findings
Summarize the major insights gained from the analysis.

---

### 9. Conclusion
Provide a brief conclusion of the project findings.

---

### 10. Faced Errors

---

### References
List any references used for the analysis.

### Timeline
