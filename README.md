
# Project Title: Exploratory Data Analysis of [Dataset Name]

## Table of Contents
* [1. EDA Overview](#eda-overview)
* [2. Dataset Description](#dataset-description)
* [3. Task Goals](#task-goals)
* [4. Installation and Setup](#installation-and-setup)
* [5. Data Exploration](#data-exploration)
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

#### Handling Duplicate Values
   - I handled the duplicated values by comparing the info of both instances side by side to check what has the correct data and to keep it, and to drop the one instance that has the wrong data.

     For the first song which is "SPIT IN MY FACE!" by ThxSoMch

  ``` python
df_spotify.loc[(df_spotify['track_name']=='SPIT IN MY FACE!')]
```

   Uses .loc to locate a specific value of a specific attribute of a dataset, the outputted side-by-side info looks like this

  ![image](https://github.com/user-attachments/assets/0b8f4be5-a150-4223-9a79-9a25f2f25499)

  ![image](https://github.com/user-attachments/assets/f4baaafa-c029-41b7-a77d-1652b8935d52)

  Upon inspection, their key and beats per minute do not match as well as other values. Upon researching, the correct key and bpm for this song



  


  

#### Handling Missing Values 
   #### For Missing Keys
   
   - I handled missing values in the key attribute by filling them with a placeholder. I did not drop them from the dataset as they aren't close to 50% missing of the entire population. By replacing the missing keys with a placeholder, valid data is retained, and it can be rejected for later consideration for data analysis related to the key attribute. An easily implemented imputation method cannot be applied here since it is a categorical value.

---

### 7. Analysis

#### Overview of Dataset
- Describe the dataset structure and general information.

#### Basic Descriptive Statistics
- Provide descriptive statistics such as mean, median, and standard deviation for relevant columns.

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
