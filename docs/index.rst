FindYourProfessor Documentation
===============================

Welcome to the documentation for FindYourProfessor, a Python-based web scraper tool to extract emails of professors matching with keywords representing professors' fields of interest. It takes in the faculty directory URL of a university and keywords as input and returns the name, email, and webpage of professors with matching research interests.

Inputs
------

To use FindYourProfessor, provide the webpage URL from where you wish to find details about professors, along with the keywords.

Installation
------------

To install FindYourProfessor, you can use pip:

.. code-block:: bash

   pip install FindYourProfessor==0.2

Importing
---------

Once installed, you can import the necessary functions:

.. code-block:: python

   from FindYourProfessor.scraper import extract_urls, scrape_urls, extract_emails

User Guide
----------

To find professors matching specific keywords:

1. Provide the primary URL of the faculty directory:

.. code-block:: python

   primary_url = 'https://www.stevens.edu/school-engineering-science/departments/civil-environmental-ocean-engineering/faculty'

2. Specify the keywords representing professors' research interests:

.. code-block:: python

   keywords = ['water quality', 'machine learning', 'remote sensing', 'hydrology']

3. Extract URLs from the primary URL:

.. code-block:: python

   urls = extract_urls(primary_url)

4. Call the function to scrape URLs, search for keywords, and extract emails:

.. code-block:: python

   scrape_urls(urls, keywords)

Developed By Swaranjit Roy
--------

