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
