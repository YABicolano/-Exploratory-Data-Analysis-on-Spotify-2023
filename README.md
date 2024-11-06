# Spotify 2023 Exploratory Data Analysis

The 2023 Spotify dataset is the subject of this project's thorough Exploratory Data Analysis . Examining patterns, trends, and insights in Spotify's dataset with an emphasis on the various factors that influence its system is the aim. This project involves a detailed analysis of a dataset containing information on various tracks, including data like release year, streaming numbers, and musical characteristics. The following content below explore key questions and conduct comprehensize data analysis to have a better understanding of the patters, insights, and correlations in the data set.

## Overview of Dataset w/ Code blocks

1. **How many rows and columns does the dataset contain?**
```python
rxc=df.shape
print("Rows x Column:",rxc)
```
- The dataset contains 953 rows and 24 columns.
2. Data Type of Each Column
```python
type=df.dtypes
type
````
- The data types are either Integers or Objects
 
3. Check for Null Values
```python
print(df.isnull().sum())
```

## Basic Descriptive Statistics

1. **What are the mean, median, and standard deviation of the `streams` column?**
   - The code block below converts the column into numeric and calculates the mean, median and std.
```python
# Filter out rows where 'streams' is not numeric by converting it to float
meanvalue = pd.to_numeric(df['streams'], errors='coerce').dropna().mean()
medvalue = pd.to_numeric(df['streams'], errors='coerce').dropna().median()
stdvalue = pd.to_numeric(df['streams'], errors='coerce').dropna().std()
#display output in two decimal places
print("Mean:", round(meanvalue,2))
print("Median:", round(medvalue,2))
print("STD:", round(stdvalue,2))
```
Output:  
Mean: 514137424.94  
Median: 290530915.0  
STD: 566856949.04
  
2. **What is the distribution of `released_year` and `artist_count`?**
   - The code block below displays a bar graph of the distributions of artist count and released year.
   
Input:

```python
# Plot graph of 'released_year' 
sns.displot(df, x="released_year", bins=20, kde=True, height=4, aspect=1.5).set(
    title="Distribution of Released Year", xlabel="Released Year")
    
# Plot of 'artist_count' 
sns.displot(df, x="artist_count", bins=10, kde=True, height=4, aspect=1.5).set(
    title="Distribution of Artist Count", xlabel="Artist Count")
```
Output:  

![Distribution Bar graph](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image1.png)  
 
Released Year graph showcase the number of releases that has spiked dramatically since 2000 and has stayed relatively steady around 2020. This is probaly due to the effect of the progress of digital platforms and their usage in musical streaming platforms. This suggests music distribution can be disseminated to wider audiences with much greater ease over the recent years.The  Artist Count graph suggest that majority of the tracks are from a single artist, although that percentage drops very dramatically for more than one artist. It might suggest a preference for single artist projects, in which most of the album's tracks are performed only by one artist.
## Top Performers

1. **The Top 5 Most Streamed Tracks.**
   - The code block below arranges the dataframe of the 'streams' column in descending order and utilizes .head() syntax to display the top 5
               
Input:
```python
top5 = df.sort_values('streams', ascending=False).head()
top5
```
Output:
![Top5Tracks](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image2.png)  
    This table shows popular songs with large streaming counts and major playlist entries on Spotify and Apple Music. The Weeknd's 2019 single "Blinding Lights" has received over 3.7 billion streams and is featured on 43,899 Spotify playlists. Other popular songs include Ed Sheeran's "Shape of You" and Lewis Capaldi's "Someone You Loved," both of which have a large number of streams and appear on numerous playlists. These tracks, largely published between 2017 and 2019, illustrate recent streaming trends that have favored specific successful songs. frequency in recent years, most likely impacted by music streaming platforms.
    
   
2. **The Top 5 Artists with Most Tracks Released**
   
Input:
```python
top_ntracks = df['artist(s)_name'].value_counts().head()
top_ntracks 
```
Output:  
## Top 5 Most Frequent Artists

| Artist        | Track Count |
|---------------|-------------|
| Taylor Swift  | 34          |
| The Weeknd    | 22          |
| Bad Bunny     | 19          |
| SZA           | 19          |
| Harry Styles  | 17          |



## Temporal Trends

1. **Analysis on the trends in the number of tracks released over time.**
   
Input:
```python
import seaborn as sns
# Compute number of tracks released per year
year = df['released_year'].value_counts().reset_index()
year.columns = ['released_year', 'num_tracks']  # Rename columns for clarity
print(year)

# .max to locate the year with highest number of releases
myear = year.loc[year['num_tracks'].idxmax(), 'released_year']
mtracks = year['num_tracks'].max()

# Display output
print(f"Year {max_year} with {max_tracks} tracks")


# Plot a line graph using Seaborn 
sns.relplot(x='released_year', y='num_tracks', kind='line', data=track_year, height=4, aspect=2, marker="o" ).set( 
    title='Number of Tracks Released Per Year',xlabel='Year', ylabel='Number of Tracks')     
```
Output:  

![Topannually](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image3.png)  
    The number of tracks released annually is shown in the line graph above, which shows a comparatively small and consistent output from the 1940s to the early 2000s. The amount of songs published, however, noticeably increases starting in the mid-2000s and  evidently increases in the 2010s. The number of releases increases dramatically by 2020, reaching a peak of more than 400 tracks in a single year. This trend points to a significant increase in the rate of music production and release in recent years, which was probably impacted by streaming services and platforms.
   

3. **The Top Tracks Released Per Month**
   
Input:
```python
# Calculate the number of tracks released per month and sort 
trackmonth = df['released_month'].value_counts().reset_index()
trackmonth.columns = ['released_month', 'num_tracks']
trackmonth = trackmonth.sort_values(by='released_month').reset_index(drop=True)

# Display months in an organized way for user readability
months = {1: "January", 2: "February", 3: "March", 4: "April", 5: "May", 
          6: "June", 7: "July", 8: "August", 9: "September", 10: "October", 
          11: "November", 12: "December"}
trackmonth['name_month'] = trackmonth['released_month'].map(months)

# Identify the month with the highest release count
maxmonth = trackmonth.loc[trackmonth['num_tracks'].idxmax(), 'name_month']
maxtracks = trackmonth['num_tracks'].max()
print(pd.DataFrame({'Month with highest release': [maxmonth], 'Number of tracks': [maxtracks]}))

# Set plot using Seaborn syntaxes
sns.catplot(x='name_month', y='num_tracks', kind='bar', data=track_month, height=6, aspect=1.5, order=track_month['name_month']).set_axis_labels("Month", "Number of Tracks").set_titles("Number of Releases per Month")

```
Output:  
![Topmothly](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image4.png)  
This bar chart illustrates the monthly distribution of track releases over the year. May has the highest number of releases, surpassing 120 tracks, while January and June also show high release counts. August, however, has the lowest number of releases, hinting at seasonal trends that occur in the music industry. The data suggests that artists and record labels may prefer specific months for releasing new music, potentially for strategic reasons.
## Genre and Music Characteristics

1. **The Correlation Between Streams and Musical Attributes**

Input:
```python
# Calculate the correlation matrix for selected columns in the DataFrame
correlation = df[['streams', 'danceability_%', 'valence_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']].corr()

# Sort the correlation values of the streams
cor_streams = correlation['streams'].sort_values(ascending=False)
print("Correlation to streams")
print(cor_streams)

# Display heatmap  using Seaborn
sns.heatmap(correl, annot=True, cmap="inferno", fmt=".1g", linewidths=0.5)

```
Output:  
#### Correlation to Streams

| Feature            | Correlation |
|--------------------|-------------|
| streams            | 1.000000    |
| acousticness_%     | -0.004485   |
| energy_%           | -0.026051   |
| valence_%          | -0.040831   |
| instrumentalness_% | -0.044902   |
| liveness_%         | -0.048337   |
| danceability_%     | -0.105457   |
| speechiness_%      | -0.112333   |  

#### Heatmap
![Heatmap](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image5.png)  
This heatmap depicts the relationship between several musical variables from the Spotify dataset. Notably, there is a moderate positive correlation (0.4) between valence_% and danceability_%, which suggests that happy songs are more danceable. There is also a large negative relationship (-0.6) between energy_% and acousticness_%, implying that high-energy songs are generally less acoustic. The other attributes have lesser correlations, indicating less direct linkages in this sample.

3. **Correlation between Danceabilit and Energy, and the Correlation Between Valence and Acousticness**
   
Input:
```python
# Calculate the correlation coefficients directly
danceability_energy_corr = df['danceability_%'].corr(df['energy_%'])
valence_acousticness_corr = df['valence_%'].corr(df['acousticness_%'])

# Print the correlation coefficients
print(f"Correlation between danceability_% and energy_%: {danceability_energy_corr:}")
print(f"Correlation between valence_% and acousticness_%: {valence_acousticness_corr:}")
```
Output:  

| Attributes                      | Correlation Value |
|---------------------------------|-------------------|
| danceability_% and energy_%      | 0.1985           |
| valence_% and acousticness_%     | -0.0812          |


## Platform Popularity

1. **Comparison of Numbers of Tracks in `spotify_playlists`, `spotify_charts`, and `apple_playlists`**
   
Input:
```python
#Create a barplot using seaborn
bar_plot = sns.barplot(x=num_tracks_playlists.index, y=num_tracks_playlists.values)

# Setting the title and labels using Seaborn
bar_plot.set_title("Number of Tracks in Each Platform")  
bar_plot.set_xlabel("Category")                          
bar_plot.set_ylabel("Number of Tracks")                 
bar_plot.set(yscale='log')                               

# Retrieve and display the Top 5 tracks with their respective counts in the playlists
#convert to numeric types and sum the total tracks in each category.
numplaylists = df[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].apply(pd.to_numeric, errors = 'coerce').sum()
print("Number of tracks in each Platform")
print(numplaylists)

#Top 5 tracks with their respective count in the playlists
df.loc[[55, 179, 86, 620, 41], ['track_name', 'artist(s)_name', 'in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].head()
```
Output:  
![Bargraph](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image6.png)  

This chart shows the distribution of popular songs across several music platforms, with Spotify playlists having more popular songs than Deezer and Apple playlists. Spotify has considerably more tracks than the other two services, as indicated by the bar height difference. Deezer playlists feature fewer songs than Spotify, but more than Apple playlists. This shows that Spotify has a substantially larger collection or more selected playlists than Deezer and Apple in this sample.


## Advanced Analysis

1. **Patterns among tracks with the same key or mode (Major vs. Minor)**
   
Input:
```python
# Calculate the mean streams for each mode and key
streamdata = df.groupby(['mode', 'key'])['streams'].mean().reset_index()

# Arrange the data descendingly based on average streams
sortstream = streamdata.sort_values(by='streams', ascending=False)

# Print average streams by mode and key
print("Average streams by mode and key\n", sorted_stream)

# Create bar plot using Seaborn
bar_plot = sns.barplot(x='key', y='streams', hue='mode', data=sorted_stream, palette='cividis')

# Set the labels and title
bar_plot.set_xlabel("Key")                             
bar_plot.set_ylabel("Average Streams")                 
bar_plot.set_title("Patterns among tracks with key and mode by average")  
bar_plot.legend(title='Mode')
```
Output:  
![Bargraph](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image7.png)  
The bar graph illustrates the average number of streams for tracks categorized by key and mode (Major or Minor). The y-axis represents the average streams in the range of 0 to 7 x 10^8, while the x-axis lists the musical keys from E to A. Major keys generally have higher average streams compared to Minor keys, with the key of E Major having the highest average streams at over 7 x 10^8. This graph is relevant as it highlights the popularity of different musical keys and modes, potentially guiding musicians and producers in their creative decisions.

3. **Genres or Artists that Frequently Appear on Charts**
   
Input:
```python
# Convert columns to numeric, coercing errors to NaN
converted = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists','in_apple_charts', 
             'in_deezer_playlists', 'in_deezer_charts', 'in_shazam_charts']

df[converted] = df[converted].apply(pd.to_numeric, errors='coerce')

# Arrange and organize the appearances of artists
appear = df.groupby('artist(s)_name')[converted].sum()
appear['Total_appearances'] = appear.sum(axis=1)
sorted_appear = appear.sort_values(by='Total_appearances', ascending=False).reset_index()

# Create a bar plot using Seaborn
barplot = sns.barplot(x='artist(s)_name', y='Total_appearances', hue='artist(s)_name',data=sorted_appear.head(), palette='cividis', legend=False)

# Setting the title and labels using Seaborn
barplot.set_title("Top 5 Artists in All Playlists Provided")  
barplot.set_ylabel("Total Appearances")                      
barplot.set_xlabel("Artists")      
```
Output:  
![Bargraph](https://github.com/YABicolano/-Exploratory-Data-Analysis-on-Spotify-2023/blob/5ade615f8864f01e0b68fb54238de51fe9418bc1/Images/image8.png)  
The bar chart titled "Top 5 Artists in All Playlists Provided" showcases the total appearances of five artists in playlists. The Weeknd leads with approximately 145,000 appearances, followed by Taylor Swift with around 135,000. Ed Sheeran and Harry Styles have about 130,000 and 115,000 appearances respectively, while Eminem rounds out the top five with roughly 100,000 appearances. This chart highlights the significant popularity and widespread appeal of these artists based on their frequency in playlists.


---

## Contents of the Repository

  - `spotify-2023.csv`: Contains the raw dataset with track metadata and streaming statistics.
  - `EDA_spotify.ipynb`: Notebook with analysis code to answer the above questions.
    


## About the Author

My name is Yule Andre R. Osea an Electronics Engineering Student in University of Santo Tomas. This Exploratory Data Analysis was conducted for educational purposes and to enhance my coding skills for future benefit. If there are queries regarding my code and repository, feel free to message me.



