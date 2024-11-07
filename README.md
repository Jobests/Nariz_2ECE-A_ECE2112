# Exploratory Data Analysis on Spotify 2023

## Description
#### The project aims to perform Exploratory Data Analysis(EDA) on a dataset of the most streamed songs on spotify in 2023.

## Libraries Needed
* Pandas
* Matplotlib
* Seaborn

## Loading the Dataset

#### The code reads the spotify_2023.csv file using the `pd.read_csv` function into a Pandas DataFrame named `spotify`, using ISO-8859-1 encoding to handle special characters.

```python
spotify = pd.read_csv('spotify_2023.csv', encoding='ISO-8859-1')
spotify
```

## Output
![Screenshot 2024-11-05 191913](https://github.com/user-attachments/assets/c021a06d-e396-4b68-a5e3-80035a7905d8)

## OVERVIEW OF THE DATASET

## Identifying the number of rows and column

#### This code retrieves the dimensions of the `spotify` DataFrame using the `spotify.shape` attribute, which returns a tuple containing the number of rows and columns; it unpacks this tuple into the variables rows and columns. The subsequent print statements then output the total number of rows and columns in the DataFrame, providing a quick overview of its size.

```python
rows, columns = spotify.shape
print('The number of rows are:', rows)
print('The number of columns are:', columns)
```
## Output

#### The output indicates that the `spotify` DataFrame has 953 rows and 24 columns, meaning there are 953 records in the dataset, each with 24 attributes or features. This provides a summary of the dataset's size and structure, showing the number of entries and the various pieces of information available for each entry.

```output
The number of rows are: 953
The number of columns are: 24
```

## Identifying the data types of each column

#### This code retrieves the data types of each column in the `spotify` DataFrame using `spotify.dtypes` and assigns them to the variable `data_types`. It then prints a message followed by the `data_types` Series, providing a quick overview of the type of data stored in each column, such as integers, floats, or strings.

```python
data_types = spotify.dtypes
print('Data types of each columns are:')
data_types
```
## Output

#### This is the output showing the data types of each column in the `spotify` DataFrame, indicating a mix of `object` and `int64` types. This summary provides insight into the nature of the data in the dataset.

```output
Data types of each column are:
track_name              object
artist(s)_name          object
artist_count             int64
released_year            int64
released_month           int64
released_day             int64
in_spotify_playlists     int64
in_spotify_charts        int64
streams                 object
in_apple_playlists       int64
in_apple_charts          int64
in_deezer_playlists     object
in_deezer_charts         int64
in_shazam_charts        object
bpm                      int64
key                     object
mode                    object
danceability_%           int64
valence_%                int64
energy_%                 int64
acousticness_%           int64
instrumentalness_%       int64
liveness_%               int64
speechiness_%            int64
dtype: object
```
## Looking for missing values/duplicated values

```python
# Calculate the number of missing values in each column
missing_values = pd.isnull(spotify).sum()
print('The columns that have missing values are:')
print(missing_values[missing_values > 0])

# Find duplicate rows based on 'track_name' and 'artist(s)_name'
duplicate_rows = spotify[spotify.duplicated(['track_name', 'artist(s)_name'])]
print('Rows that have duplicate values are:')
duplicate_rows
```

## Output

#### The columns `in_shazam_charts` and `key` have 50 and 95 missing values, respectively. This suggests that some data related to song charts and key signatures are incomplete and require cleaning.

```output
The columns that have missing values are:
in_shazam_charts    50
key                 95
dtype: int64
```
#### these tracks are performed by ThxSoMch, The Weeknd, Lizzo, and Rosa Linn, respectively. These rows have duplicate track names but differ in other aspects such as playlist or chart appearances and streaming counts.

```output
Rows that have duplicate values are: 
```
![Screenshot 2024-11-06 232516](https://github.com/user-attachments/assets/e786c413-8e68-4945-969a-11aecddcf52d)


## Cleaning the Data
* Handling missing values
* Removing duplicate values
* Correcting data types

#### The code removes missing values from specified columns, drops duplicates, converts the `streams` column to numeric, and saves the cleaned DataFrame as `spotify_cleaned_2023.csv` for further analysis.

```python
# Drop rows with missing values in 'in_shazam_charts' and 'key'
spotify.dropna(subset=['in_shazam_charts', 'key'], inplace=True)

# Drop duplicate entries based on 'track_name' and 'artist(s)_name'
spotify.drop_duplicates(subset=['track_name', 'artist(s)_name'], inplace=True)

# Convert 'streams' to numeric, coercing errors to NaN
spotify['streams'] = pd.to_numeric(spotify['streams'], errors='coerce')

# Drop rows with missing values in 'streams' after conversion
spotify.dropna(subset=['streams'], inplace=True)

# Save the cleaned DataFrame to a new CSV file
spotify.to_csv('spotify_cleaned_2023.csv', index=False)

# Load the cleaned data
spotify_cleaned = pd.read_csv('spotify_cleaned_2023.csv', encoding='ISO-8859-1')
spotify_cleaned
```
## Output

#### The cleaned data contains 813 rows and 24 columns, indicating that rows with missing values or duplicates were successfully removed while retaining the original structure of the dataset.

![Screenshot 2024-11-05 213526](https://github.com/user-attachments/assets/9af98e3f-f233-4efb-81e6-ffaf66149f4d)

## BASIC DESCRIPTIVE STATISTICS

#### This code calculates the mean, median, and standard deviation of the `streams` column in the cleaned dataset. The results provide insights into the average number of streams, the midpoint of the data, and the variability of stream counts, respectively.

```python
mean_streams = spotify_cleaned['streams'].mean()
median_streams = spotify_cleaned['streams'].median()
std_streams = spotify_cleaned['streams'].std()

print('The mean of the streams column is:', mean_streams)
print('The median of the streams column is:', median_streams)
print('The standard deviation of the streams column is:', std_streams)
```
## Output

#### This is the result of the analysis

```output
The mean of the streams column is: 468922407.2521525
The median of the streams column is: 263453310.0
The standard deviation of the streams column is: 523981505.32150424
```

## Outliers and Trends
#### The boxplot makes it easy to identify outliers in the data. Outliers are displayed as individual points outside the whiskers of the box, allowing us to quickly spot any values that are significantly different from the rest.

* `plt.figure()` sets the size of the plot to 12 inches wide and 6 inches tall for better readability.
* `sns,boxplot()` creates the boxplot using the `artist_count` data from the `spotify_cleaned` DataFrame.

```python
plt.figure(figsize=(12,6))
sns.boxplot(x=spotify_cleaned['artist_count'])
plt.grid()
plt.title('Distribution of Artist Count')
plt.xlabel('Artist Counts')
plt.show()
```
## Ouput
![image](https://github.com/user-attachments/assets/529d3bd4-517f-4a9f-87ab-e684ffe1ee30)

* `plt.figure()` sets the size of the plot to 12 inches wide and 6 inches tall for better readability.
* `sns,boxplot()` creates the boxplot using the `released_year` data from the `spotify_cleaned` DataFrame.

```python
plt.figure(figsize=(12,6))
sns.boxplot(x=spotify_cleaned['released_year'])
plt.grid()
plt.title('Distribution of Released Year')
plt.xlabel('Released Year')
plt.show()
```

## Output

![image](https://github.com/user-attachments/assets/194efd4e-7a31-4f70-a819-d47ff6adb916)

#### The histplot is useful for understanding the distribution of the `released_year` variable in the dataset. It helps identify trends in how releases have been spread across years. The KDE curve further highlights the density of releases, showing a smoothed distribution.

* `plt.figure(figsize=(12,6)):` This sets the size of the plot to 12 inches wide and 6 inches tall, providing a spacious view for the data.

* `sns.histplot(spotify_cleaned['released_year'], bins=30, kde=True, color='#17becf'):`
  * `sns.histplot:` Creates a histogram to visualize the distribution of values in the released_year column from the spotify_cleaned dataset.
  * `bins=30:` Divides the data into 30 bins, or intervals, for a detailed look at how released_year values are distributed.
  * `kde=True:` Adds a Kernel Density Estimate (KDE) curve over the histogram, which smooths the distribution and shows the underlying trend.
  * `color='#17becf':` Uses a soft teal color (#17becf) for both the histogram bars and the KDE curve, making the plot visually appealing and easy to interpret.

```python
plt.figure(figsize=(12,6))
sns.histplot(spotify_cleaned['released_year'],bins=30,kde=True, color='#17becf')
plt.grid()
plt.title('Distribution of Released Year')
plt.xlabel('Released Year')
plt.ylabel('Frequency')
plt.show()
```
## Output
![image](https://github.com/user-attachments/assets/d27ab491-763f-435b-b077-5b2be2bf9b0d)

## TOP PERFORMERS

## Highest Streamed Track

* `spotify_cleaned['streams'].idxmax():` Finds the index of the track with the highest stream count.
* `spotify_cleaned.loc[max_stream, ['track_name', 'streams']]:` Selects the track name and stream count for the track with the highest streams.

```python
max_stream = spotify_cleaned['streams'].idxmax()
highest_stream = spotify_cleaned.loc[max_stream, ['track_name', 'streams']]
highest_stream
```
## Output

#### The output shows that the track "Shape of You" has 3,562,543,890 streams, and its index in the DataFrame is 151.

```output
track_name    Shape of You
streams       3562543890.0
Name: 151, dtype: object
```

## Top 5 Stream Tracks

#### The code selects the top 5 tracks with the highest stream counts from the `spotify_cleaned` DataFrame. It first filters the `track_name` and `streams` columns, sorts the data in descending order by streams using `sort_values()`, and then retrieves the top 5 tracks with `.head()`. The result is stored in the `top_tracks` variable, showing the tracks with the most streams.

```python
top_tracks = spotify_cleaned[['track_name','streams']].sort_values(by='streams', ascending=False).head()
top_tracks
```

## Output

#### The output shows the top 5 most stream tracks in the dataset, along with the total streams and the location of each tracks. The output is the result of using `.sort_values()`

![image](https://github.com/user-attachments/assets/47a91d14-4e6f-405d-bf2b-111a9868d77a)

## Top 5 most Frequent Artist

#### This code splits the `artist(s)_name` column into individual artist names using `.str.split(', ')`. The `explode()` function then converts the list of artists into separate rows, resulting in one row per artist. The `value_counts()` function counts the occurrences of each artist, and `.head()` retrieves the top 5 most frequent artists. The result is stored in the `top_artists` variable, which shows the 5 most frequent artists in the dataset.

```python
artist_splt = spotify_cleaned['artist(s)_name'].str.split(', ')

all_artists = artist_splt.explode()

top_artists = all_artists.value_counts().head()
top_artists
```
## Output

#### This output shows the top 5 most frequent artists in the dataset, along with the number of times each artist appears. The output is the result of using `value_counts()` on the `artist(s)_name` column after splitting and exploding the artist names. The data type is `int64`, indicating that the count values are integers.

```output
artist(s)_name
Bad Bunny         36
Taylor Swift      32
The Weeknd        26
Kendrick Lamar    23
Feid              21
Name: count, dtype: int64
```

## TEMPORAL TRENDS

* `groupby('released_year'):` Groups the data by the release year of tracks.
* `size():` Counts the number of tracks in each group (i.e., the number of tracks released per year).
* `reset_index(name='track_count'):` Resets the index and names the count column as track_count.

```python
tracks_by_year = spotify_cleaned.groupby('released_year').size().reset_index(name='track_count')

plt.figure(figsize=(10, 6))
sns.lineplot(data=tracks_by_year, x='released_year', y='track_count')
plt.grid(True)
plt.xlabel("Year of Release")
plt.ylabel("Number of Tracks")
plt.title("Tracks Released Annually")
plt.show()
```
## Output

#### The graph clearly shows that the number of tracks released significantly increased starting in 2020 and continuing into the following years. This trend highlights a noticeable rise in track releases after 2020, with the data indicating that more music was produced and released in these later years compared to earlier periods. The upward shift in releases starting from 2020 is evident when comparing it to previous years.

![image](https://github.com/user-attachments/assets/64d4c941-5799-473f-888e-78c60070fc34)

* `groupby('released_month'):` Groups the data by the month of release.
* `size():` Counts the number of tracks released in each month.
* `reset_index(name='track_count'):` Resets the index and assigns the count column a name, `track_count.`

```python
tracks_by_month = spotify_cleaned.groupby('released_month').size().reset_index(name='track_count')

plt.figure(figsize=(10, 6))
sns.barplot(data=tracks_by_month, x='released_month', y='track_count', width=0.8, color='#FF6F61')
plt.grid(True)
plt.title("Tracks Released Monthly")
plt.xlabel("Month of Release")
plt.ylabel("Number of Tracks")
plt.show()
```
## Output

#### The bar graph displaying tracks released per month reveals that the highest number of songs were released in either January or May. These two months stand out, showing a significant peak in track releases compared to other months throughout the year. This pattern suggests that January and May may be key months for music releases, with a notable concentration of new tracks being made available to listeners during these times.

![image](https://github.com/user-attachments/assets/3ed1bfc6-3018-4133-b651-21c53fd108d6)


##  GENRE AND MUSIC CHARACTERISTICS

## Stream and Musical Attribute Correleation

#### This code calculates and visualizes the correlation between `streams`, `bpm`, `danceability_%`, and `energy_%` using a heatmap, showing relationships between these attributes in the `spotify_cleaned` dataset.

* `sns.heatmap(x, annot=True, cmap='RdYlGn_r'):` Creates a heatmap of the correlation matrix x. The annot=True argument displays the correlation values on the heatmap, and cmap='RdYlGn_r' applies a red-to-green reversed color gradient.

```python
x = spotify_cleaned[['streams','bpm','danceability_%','energy_%']].corr()
x

sns.heatmap(x, annot=True, cmap='RdYlGn_r')
plt.title('Correlation of Streams and Musical Attributes')
plt.show()
```

 ## Output

#### In this correlation heatmap, we can see the relationships between various musical attributes like `streams`, `bpm`, `danceability_%`, and `energy_%`. The correlation coefficient ranges from -1 to 1, where values closer to 1 or -1 indicate stronger positive or negative correlations, respectively. Values close to 0 imply a weak or no correlation.

* `BPM`: The correlation between streams and bpm is -0.025, which is very close to 0. This suggests that there is virtually no relationship between the song's tempo and the number of streams.
* `Danceability_%`: The correlation between streams and danceability_% is -0.094. This is a weak negative correlation, suggesting that higher danceability might slightly decrease streams, but the effect is minimal.
* `Energy_%`: The correlation between streams and energy_% is -0.037, which is again very close to 0. Like bpm, this indicates that energy level has little to no impact on streams.

### Conclusion

#### All the correlations between streams and the musical attributes are weak and close to zero. This implies that none of these attributes appear to have a significant influence on streams based on this data.
 
 ![image](https://github.com/user-attachments/assets/52ff96f0-f945-49f4-9cc7-0bb5dfd5d9ca)

## Correleation with the 4 Musical Attributes

#### This code calculates the correlation between `danceability_%` and `energy_%` and the `valence_%` and `acousticness_%` in the spotify_cleaned dataset. The `.corr()` computes for the correlation coefficient.

```python
y = spotify_cleaned[['danceability_%','energy_%']].corr()
sns.heatmap(y, annot=True)
plt.title('Correlation of Danceability and Energy')
plt.show()

z = spotify_cleaned[['valence_%','acousticness_%']].corr()
sns.heatmap(z, annot=True)
plt.title('Correlation between Valence and Acousticness')
plt.show()
```
## Output

#### The analysis showed that danceability and energy have a stronger correlation than valence and acousticness.

![image](https://github.com/user-attachments/assets/36d876ae-ee85-46a4-84a8-512380fdc67c)
![image](https://github.com/user-attachments/assets/b1536491-5632-40d3-ae8c-b2aac4b046fe)

## PLATFORM POPULARITY

## Popularity Platform

* `spot_track_sum = spotify_cleaned['in_spotify_playlists'].sum():` calculates the sum of all values in the in_spotify_playlists column, representing the total number of tracks in Spotify playlists.

* `deezer_track_sum = pd.to_numeric(spotify_cleaned['in_deezer_playlists'], errors='coerce').sum():` converts values in the in_deezer_playlists column to numeric, handling any non-numeric values by setting them to NaN (errors='coerce), and then sums these values to get the total number of tracks in Deezer playlists.

* `apple_track_sum = spotify_cleaned['in_apple_playlists'].sum():` calculates the sum of all values in the in_apple_playlists column, representing the total number of tracks in Apple playlists.

* `platforms = ['Spotify', 'Deezer', 'Apple']:` creates a list of platform names for labeling the chart.

* `sum_tracks = [spot_track_sum, deezer_track_sum, apple_track_sum]:` creates a list of the total track counts for each platform, to be used as data for the bar chart.

* `plt.bar(platforms, sum_tracks, color=['blue','orange','yellow']):` creates a bar chart with platforms as the x-axis labels, sum_tracks as the heights of the bars, and assigns colors to each platform’s bar: blue for Spotify, orange for Deezer, and yellow for Apple.

* `plt.grid():` adds a grid to the chart for easier readability.

* `plt.title('Number of Tracks Popularity by Platform'):` sets the chart title to "Number of Tracks Popularity by Platform".

* `plt.ylabel('Number of Tracks'):` labels the y-axis as "Number of Tracks" to indicate what the bars represent.

* `plt.show():` displays the plot.

```python
spot_track_sum = spotify_cleaned['in_spotify_playlists'].sum()
deezer_track_sum = pd.to_numeric(spotify_cleaned['in_deezer_playlists'], errors='coerce').sum()
apple_track_sum = spotify_cleaned['in_apple_playlists'].sum()

platforms = ['Spotify', 'Deezer', 'Apple']
sum_tracks = [spot_track_sum, deezer_track_sum, apple_track_sum]

plt.bar(platforms, sum_tracks, color=['blue','orange','yellow'])
plt.grid()
plt.title('Number of Tracks Popularity by Platform')
plt.ylabel('Number of Tracks')
plt.show()
```
## Output

#### The plot shows that Spotify has the most tracks, followed by Deezer, and then Apple. Specifically, the number of tracks in each platform’s playlist is 3,943,504 for Spotify, 48,719 for Deezer, and 69,618 for Apple.

![image](https://github.com/user-attachments/assets/00f9544d-9902-49c8-a470-e9d25034bd81)

## ADVANCE ANALYSIS

* `spotify_cleaned.groupby('key'):` This groups the data in spotify_cleaned by the key column, which presumably contains information about the musical key of each track.
* `keys_group.plot(kind='bar'):` This line generates a bar plot from the keys_group data

```python
keys_group = spotify_cleaned.groupby('key')['streams'].sum()

keys_group.plot(kind='bar', x='key', y='streams', title='Total Streams by Key', legend=False)
plt.grid()
plt.xlabel('Key')
plt.ylabel('Total Streams')
plt.xticks()
plt.show()
```

## Output

#### The bar graph shows that the majority of songs are in C#. Following C# in frequency are D, F, G, G#, B, E, F#, A#, A, and D# in descending order.
![image](https://github.com/user-attachments/assets/31c9ae16-6490-4aee-9e87-0f771f621138)

* `spotify_cleaned.groupby('artist(s)_name'):` This groups the data by the artist(s)_name column. This column likely contains the name(s) of the artist(s) associated with each track.
* `[['in_apple_playlists', 'in_spotify_playlists', 'in_deezer_playlists']]:` After grouping, the code selects the columns related to playlist counts on three platforms: Apple, Spotify, and Deezer. These columns track how many playlists the artist's songs appear on for each platform.
* `.sum():` This computes the sum of the playlist appearances for each artist across the selected columns `(in_apple_playlists, in_spotify_playlists, and in_deezer_playlists)`. The result is a DataFrame where each row corresponds to an artist, and the values represent the total number of playlist appearances across the three platforms.

* `.sum(axis=1):` The sum(axis=1) function computes the sum of the playlist counts across the three platforms (Apple, Spotify, and Deezer) for each artist, row by row.
* `reset_index():` This resets the index to a default integer index. After grouping, the artist names are the index, but this line converts the artist names back into a regular column for easy access.
* `head(10):` This selects the top 10 artists with the most total playlists, based on the sorted values.
* `plt.figure(figsize=(15, 6))`: Creates a figure for the plot with a specified size (15 inches wide by 6 inches tall).
  * `plt.bar():` This creates a bar plot where:
  * The x-axis represents the artist names (artist(s)_name).
  * The y-axis represents the Total Playlists for each artist.
  * color='g': The bars will be colored green.

```python
artists_playlist = spotify_cleaned.groupby('artist(s)_name')[['in_apple_playlists', 'in_spotify_playlists', 'in_deezer_playlists']].sum()

artists_playlist['Total Playlists'] = artists_playlist[['in_apple_playlists', 'in_spotify_playlists', 'in_deezer_playlists']].sum(axis=1)

artists_summary_playlist = artists_playlist[['Total Playlists']].reset_index()
artists_summary_playlist = artists_summary_playlist.sort_values(by='Total Playlists', ascending=False).reset_index(drop=True)
artisttop10_playlist = artists_summary_playlist.head(10)

plt.figure(figsize=(15, 6))
plt.bar(artisttop10_playlist['artist(s)_name'], artisttop10_playlist['Total Playlists'], color='g')
plt.title('Top 10 Artists by Total Playlists')
plt.xlabel('Artist(s) Name')
plt.ylabel('Total Playlists')
plt.grid()
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

## Output
![image](https://github.com/user-attachments/assets/5d5e32e0-b750-45fe-99d6-fd87abbf6bf0)

* The code groups the data in `spotify_cleaned` by the `artist(s)_name` column.
* It sums up the chart appearances of each artist across three platforms: Apple, Spotify, and Deezer.
* The resulting `artists_charts` DataFrame now contains the total chart appearances for each artist on each platform.
* `axis=1` ensures that the sum is calculated horizontally (across columns).
* The `reset_index(drop=True)` ensures that the index is restructured after sorting.
* The bars are colored red `(color='r')`.
* `plt.tight_layout()` ensures that everything fits nicely within the figure.
* `plt.show()` displays the final plot.


```python
artists_charts = spotify_cleaned.groupby('artist(s)_name')[['in_apple_charts', 'in_spotify_charts', 'in_deezer_charts']].sum()

artists_charts['Total Playlists'] = artists_charts[['in_apple_charts', 'in_spotify_charts', 'in_deezer_charts']].sum(axis=1)

artists_summary_charts = artists_charts[['Total Playlists']].reset_index()

artists_summary_charts = artists_summary_charts.sort_values(by='Total Playlists', ascending=False).reset_index(drop=True)

artisttop10_charts = artists_summary_charts.head(10)

plt.figure(figsize=(15, 6))
plt.bar(artisttop10_charts['artist(s)_name'], artisttop10_charts['Total Playlists'], color='r')

plt.title('Top 10 Artists by Total Charts')
plt.xlabel('Artist(s) Name')
plt.ylabel('Total Charts')
plt.grid()
plt.xticks(rotation=45)  

plt.tight_layout()
plt.show()
```

## Output
![image](https://github.com/user-attachments/assets/381c2d33-92a9-4aa8-9e14-81ce1d216f4d)

### Conclusion
#### The output indicates that Taylor Swift has consistently appeared at the top of both playlists and charts across multiple platforms (Apple, Spotify, and Deezer). Specifically, she ranks #1 based on her total appearances on both playlists and charts.

## References
* https://www.geeksforgeeks.org/
* https://stackoverflow.com/
* https://youtu.be/8d7ywKCm6HI?si=PsZ2MpOTtOxtwqyp
* https://youtu.be/xi0vhXFPegw?si=TzFIvmuk84dzatbZ
* https://youtu.be/Liv6eeb1VfE?si=bh3VcDjWzYeFfJAw
* https://www.freecodecamp.org/
* https://www.codecademy.com/
* https://www.statology.org/
