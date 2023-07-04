# Data-cleaning
The Data Cleaning repository is designed as a companion project to the GitHub Scraper repository (located at https://github.com/SAAD-BEN/github-scrapping). It focuses on processing and cleaning the data extracted from GitHub repositories using the scraping script.
### Missing Values Treatment

1. We loaded the data from the CSV file `repositories2023-01-01_2023-06-30_100PerDay.csv` using the pandas library.
2. We identified the columns of the DataFrame: `name`, `url`, `description`, `stars`, `created_at`, `language`, `forks`, `watchers`, `open_issues`, and `owner`.
3. We checked the percentage of missing values for each column. The `name`, `description`, and `language` columns contained missing values.
4. For the `name` column, we filled in the missing values using the part of the URL that follows the last '/'. This allowed us to obtain repository names for the missing values.
5. For the `description` column, we used a `generate_description()` function to deduce a provisional description from the name of each repository. The function splits the name into words using hyphens and capital letters, then creates a sentence with the wording "repository of [words from the name]".
6. We applied the `generate_description` function to the rows of the DataFrame that had a missing value in the `description` column.
7. After filling in the missing values, we checked the percentage of missing values per column again to confirm that all columns have been processed.
8. Display the rows with missing values in the `language` column.
9. Define a `get_most_common_file_type(repo_url)` function to retrieve the most common file type from the files of a GitHub repository. This function sends a request to the GitHub API to get the repository files, then counts the file extensions and returns the most common file type.
10. Create a DataFrame `no_language_repos` containing the rows with missing values in the `language` column.
11. Uncomment the following section if you are interested in file extensions in your analysis:

```python
# # Apply the get_most_common_file_type function to the "url" column of rows with missing values in the "language" column
# df.loc[df['language'].isnull(), 'language'] = df.loc[df['language'].isnull(), 'url'].apply(get_most_common_file_type)
# # Display the rows with indexes from "no_language_repos"
# df.loc[no_language_repos.index].head()
```

12. Remove the rows with missing values in the `language` column using the `dropna` method of pandas.
13. Check the information of the updated DataFrame using the `info()` method to confirm that the rows have been successfully removed.

### Duplicate Values Treatment

1. Find duplicate rows using the `duplicated` method of pandas with the `url` column as the criterion. If no duplicate rows are found, the resulting DataFrame will be empty.

```python
df[df.duplicated(subset=['url'])]
```

### Translation of Description Language

1. Define a `translate_text(text)` function to translate text to English using the Google Cloud Translation API. Make sure to obtain a valid API key and replace it in the code.
2. Apply the `translate_text` function to the `description` column of the DataFrame to translate the descriptions into English. Use the `apply` method of pandas.
3. Check the last rows of the DataFrame to see the translated descriptions.

```python
df['description'] = df['description'].apply(translate_text)
df.tail()
```

4. Save the cleaned DataFrame to a CSV file using the `to_csv` method.

```python
df.to_csv('repositories2023-01-01_2023-06-30_100PerDay_cleaned.csv', index=False)
```

Make sure to uncomment the relevant part of the code if you want to execute the steps related to file extensions or translation of descriptions.