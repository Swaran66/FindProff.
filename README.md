Function: `extract_emails(text)`
===============================

Purpose:
--------
Defines a function named `extract_emails` that extracts email addresses from a given text string.

Regular Expression (Regex):
---------------------------
- `re.findall`: Finds all occurrences in `text` that match the email pattern.
- **Pattern Explanation**:
  - `\b`: Matches word boundaries.
  - `[A-Za-z0-9._%+-]+`: Matches email username characters.
  - `@`: Matches the "@" symbol.
  - `[A-Za-z0-9.-]+`: Matches domain name characters.
  - `\.`: Matches a literal dot.
  - `[A-Z|a-z]{2,}`: Matches the top-level domain (e.g., .com, .edu).
  
Return Statement:
-----------------
- Returns a list of email addresses found in `text`.

---

Function: `extract_urls(primary_url)`
=====================================

Purpose:
--------
Extracts valid absolute URLs from a primary URL.

Steps:
------
1. Sending a GET Request:
   - Sends a GET request to `primary_url` using `requests.get()`.

2. Checking the Response Status:
   - Checks if the request was successful (status code 200).

3. Parsing HTML Content:
   - Parses the HTML content of `primary_url` using BeautifulSoup.

4. Finding `<a>` Tags with `href` Attributes:
   - Finds all `<a>` tags with `href` attributes that contain URLs.

5. Extracting and Filtering URLs:
   - Iterates through each `<a>` tag, extracts URLs, converts relative URLs to absolute using `urljoin()`, and filters valid URLs starting with `http://` or `https://`.

6. Return Statement:
   - Returns a list of valid absolute URLs extracted from `primary_url`.

---

Function: `scrape_urls(urls, keywords)`
========================================

Purpose:
--------
Scrapes web pages from provided `urls`, searches for `keywords` within each page, and extracts email addresses if keywords are found.

Steps:
------
1. Initialization:
   - Initializes an empty set `visited_urls` to track processed URLs.

2. Iterating Over URLs:
   - Iterates through each URL in `urls`, marking each URL as visited in `visited_urls` to avoid redundant processing.

3. Fetching Webpage Content:
   - Sends a GET request to each `url`, checks if the request is successful, retrieves HTML content, and parses it using BeautifulSoup.

4. Extracting Page Title:
   - Extracts the title of the webpage or assigns "No title" if no `<title>` tag is found.

5. Searching for Keywords:
   - Searches for each `keyword` in the webpage content. Adds keywords found to `found_keywords`.

6. Extracting Email Addresses:
   - Calls `extract_emails()` function to extract email addresses from the HTML content.

7. Printing Results:
   - If keywords are found (`found_keywords`), prints URL, title, found keywords, and extracted emails (using `set()` to remove duplicates).

8. Error Handling:
   - Handles exceptions during URL fetching or processing, such as HTTP request errors (`requests.exceptions.RequestException`) or unexpected errors (`Exception`).

Usage Example:
--------------
- Extracts URLs from `primary_url` and calls `scrape_urls()` with extracted URLs and keywords. Scrapes each URL, searches for specified keywords, and prints relevant details for pages that match the criteria.
