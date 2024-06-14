# FindProff.
The script starts by taking keywords and a university URL retrieves linked URLs, converts them into a BeautifulSoup object, and extracts URLs where keywords are found, alongside professors' names and email addresses.


# def extract_emails(text):
•	def extract_emails(text):: This defines a function named extract_emails that takes a single parameter, text, which is expected to be a string containing the content from which email addresses need to be extracted.
# Regular Expression (Regex)
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
This looks for strings of format like this: xyz@xyz.com or .edu etc
•	re.findall: This is a function from Python's re module (which stands for regular expressions). It searches the text for all occurrences that match the specified pattern and returns them as a list.
•	Pattern Explanation:
o	\b: A word boundary. It ensures that the match is at the start or end of a word.
o	[A-Za-z0-9._%+-]+: Matches one or more characters that are common in email usernames, including letters (both uppercase and lowercase), digits, dots (.), underscores (_), percent signs (%), plus signs (+), and hyphens (-).
o	@: Matches the "@" symbol, which is a mandatory part of every email address.
o	[A-Za-z0-9.-]+: Matches one or more characters for the domain name, including letters, digits, dots, and hyphens.
o	\.: Matches a literal dot (.), which separates the domain name and the top-level domain (TLD).
o	[A-Z|a-z]{2,}: Matches 2 or more characters, which are the top-level domain (e.g., .com, .edu, .org). The A-Z and a-z ensure the TLD is alphabetic.
## Return Statement
return emails
•	return emails: This statement returns the list of email addresses found in the text.

# Primary URL to extract URLs from
primary_url = 'https://www.stevens.edu/school-engineering-science/departments/civil-environmental-ocean-engineering/faculty'

# Keywords to search for
keywords = ['water quality', 'machine learning', 'remote sensing', 'hydrology']

•	Defines Keywords and primary that will taken as input in the next functions







***def extract_urls(primary_url):

•	Function to extract valid absolute URLs from a primary URL 
Sending a GET Request:
response = requests.get(primary_url)
•	This line sends a GET request to the primary_url provided using the requests library in Python. requests.get() retrieves the content from the URL specified by primary_url.
Checking the Response Status:
if response.status_code == 200:
•	Here, we check if the response from the GET request was successful (status_code == 200). HTTP status code 200 indicates that the request has succeeded.
Parsing HTML Content:
soup = BeautifulSoup(response.content, 'html.parser')
•	BeautifulSoup is a library used for parsing HTML and XML documents. response.content contains the raw HTML content fetched from primary_url. We initialize BeautifulSoup with 'html.parser' to parse this content.
Finding <a> Tags with href Attributes:
links = soup.find_all('a', href=True)
•	This line finds all <a> tags (<a> is for anchor tags in HTML) that have an href attribute (href=True). These tags typically contain hyperlinks.
Extracting and Filtering URLs:
urls = []
for link in links:
    url = link.get('href')
    if url:
        absolute_url = urljoin(primary_url, url)
        if absolute_url.startswith('http://') or absolute_url.startswith('https://'):
            urls.append(absolute_url)
•	In this loop, we iterate over each <a> tag found in links. We extract the value of the href attribute (url = link.get('href')).
•	We then use urljoin(primary_url, url) to convert any relative URLs (url) found within the <a> tag to absolute URLs (absolute_url), based on the primary_url.
•	We check if absolute_url starts with either http:// or https:// to ensure it's a valid absolute URL. If it is, we add it to the urls list.
Returning the List of Extracted URLs:
return urls
•	Finally, the function returns the urls list containing all valid absolute URLs found on the page.


***scrape_urls(urls, keywords):
The function `scrape_urls(urls, keywords)` scrapes web pages from the given `urls`, searches for `keywords` within each page, and extracts any found email addresses, printing relevant details for each page meeting the criteria.
Initialization:
visited_urls = set()  # Set to keep track of visited URLs
•	This initializes an empty set visited_urls to keep track of URLs that have already been processed to avoid redundant processing.
Iterating Over URLs:
for url in urls:
if url not in visited_urls:
visited_urls.add(url)  # Mark URL as visited
•	It iterates through each URL in the urls list. Before processing each URL, it checks if the URL has already been visited (if url not in visited_urls). If not visited, it adds the URL to visited_urls to mark it as visited.
Fetching Webpage Content:
try: 
response = requests.get(url)
if response.status_code == 200:
html = response.text
soup = BeautifulSoup(html, 'html.parser')
•	It sends a GET request to the current url using requests.get() and checks if the response status code is 200 (successful response).
•	If successful, it retrieves the HTML content of the page (response.text) and parses it using BeautifulSoup (soup = BeautifulSoup(html, 'html.parser')).
Extracting Page Title:
title = soup.title.text.strip() if soup.title else "No title"
•	It extracts the title of the webpage using soup.title.text. If no <title> tag is found (soup.title is None), it assigns "No title" to title.
Searching for Keywords:
found_keywords = []
for keyword in keywords:
if soup.body and soup.body.find_all(string=re.compile(r'\b{}\b'.format(re.escape(keyword))), recursive=True):
found_keywords.append(keyword)
•	It checks each keyword in the keywords list against the page content. It uses soup.body.find_all() to search for occurrences of each keyword (re.compile(r'\b{}\b'.format(re.escape(keyword)))) within the <body> of the HTML content.
•	If a keyword is found, it appends it to the found_keywords list.
Extracting Email Addresses:
emails_in_page = extract_emails(html)
•	It calls a function extract_emails() (not shown, presumably a function to extract emails from the HTML content) to extract email addresses found on the page.
Printing Results:
if found_keywords:
print(f"URL: {url}")
print(f"Title: {title}")
print(f"Keywords found: {', '.join(found_keywords)}")
print(f"Emails found: {', '.join(set(emails_in_page))}")  # Use set() to remove duplicates
print("---------------------------------------------")
•	If any keywords are found (if found_keywords), it prints:
	URL of the page
	Title of the page
	List of keywords found
	List of emails found (using set() to remove duplicates)
Error Handling:
except requests.exceptions.RequestException as e:
# Handle request exceptions
print(f"Error fetching URL: {url}")
print(e)
except Exception as e:
# Handle other unexpected exceptions
print(f"Error processing URL: {url}")
print(e)
•	It handles exceptions that may occur during the process of fetching or processing the URL. requests.exceptions.RequestException handles various HTTP request errors, and a generic Exception handler is used for any unexpected errors.
Usage Example:
# Extract URLs from primary URL
urls = extract_urls(primary_url)

# Call the Function to scrape URLs, search for keywords, and extract emails
scrape_urls(urls, keywords)
•	First, it extracts all URLs from the primary_url using the extract_urls() function.
•	Then, it calls scrape_urls() with the extracted URLs and the keywords list. This function iterates over each URL, fetches the content, searches for specified keywords, and extracts email addresses if found, printing the results for each page that matches the criteria.



