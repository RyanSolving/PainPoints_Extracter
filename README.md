# Woolworths Customer Review Analysis & Sentiment Mining

## Project Overview

This project focuses on scraping customer reviews for Woolworths stores (specifically targeting VIC stores initially) from Google Reviews. The primary goal is to collect, store, and analyze this feedback to understand customer sentiment, identify key topics of discussion (both positive and negative), and uncover trends and associations within the review data over time. The analysis employs Natural Language Processing (NLP) techniques and data visualization to extract actionable insights.

## Objective

*   Develop a web scraper to systematically collect Google Reviews for specified Woolworths stores.
*   Store the scraped review data persistently in an SQLite database.
*   Clean and preprocess the textual review data for NLP tasks.
*   Analyze overall customer sentiment based on review ratings.
*   Identify frequently mentioned topics (aspects) using noun and adjective extraction.
*   Perform aspect-based sentiment analysis to determine sentiment towards specific topics (e.g., staff, service, products).
*   Analyze trends in review topics and sentiment over different years.
*   Discover significant associations between review topics (nouns) and descriptive terms (adjectives) using Pointwise Mutual Information (PMI).
*   Visualize key findings through charts, word clouds, heatmaps, and network graphs.

## Data Source

*   **Primary Data:** Live customer reviews scraped from Google Reviews using Python (`requests` and `BeautifulSoup`).
*   **Seeding Data:** `store_list.csv` (containing initial store details and Google Review URLs).
*   **Data Storage:** SQLite Database (`wwreviews.db`).

## Analytical Process & Methodology

The project followed these key stages:

1.  **Database Setup & Initial Data:**
    *   Established a connection to an SQLite database (`wwreviews.db`).
    *   Defined and created database tables (`stores_list`, `store_reviews`) to store store information and review details respectively.
    *   Loaded initial store data from `store_list.csv`.

2.  **Web Scraping & Data Collection:**
    *   Developed Python functions to fetch and parse Google Review HTML content using `requests` and `BeautifulSoup`.
    *   Implemented logic to handle Google's dynamic loading/pagination (async URLs, next page tokens).
    *   Incorporated politeness mechanisms (User-Agent randomization, `time.sleep` delays) to respect the source server.
    *   Built error handling (`try...except`) and logging (`crawling_logs.csv`) to manage the scraping process.
    *   Extracted relevant review fields: Review ID (generated UUID), Store Number, Store Name, User Info, Rating, Review Text (snippet and full), Review Time.
    *   Persisted scraped data into the `store_reviews` table in the SQLite database, skipping already processed stores.

3.  **Data Loading & Preprocessing (Analysis Phase):**
    *   Loaded review data from the SQLite database into Pandas DataFrames.
    *   Removed duplicate reviews based on full review text.
    *   Performed text cleaning: lowercasing, removing special characters (while preserving relevant punctuation for sentiment), normalizing whitespace.
    *   Applied NLP preprocessing using `nltk` and `spacy`:
        *   Tokenization (word and sentence).
        *   Part-of-Speech (POS) tagging.
        *   Lemmatization using `WordNetLemmatizer` guided by POS tags.
    *   Filtered out very short or irrelevant reviews based on processed text length.

4.  **Feature Engineering:**
    *   Extracted the 'Year' from the 'TimeReview' field to enable temporal analysis.

5.  **Sentiment Analysis & Aspect Extraction:**
    *   Analyzed overall sentiment distribution using review 'Rating'.
    *   Extracted key aspects (nouns) and descriptors (adjectives) from processed reviews using spaCy's POS tagging.
    *   Calculated aspect-based sentiment using `nltk.sentiment.SentimentIntensityAnalyzer` (VADER) applied to sentences containing specific nouns/adjectives.
    *   Visualized the frequency of common nouns and adjectives in positive and negative reviews using `WordCloud`.

6.  **Trend & Association Analysis:**
    *   Analyzed how the frequency of specific keywords (nouns and adjectives) changed over the years, particularly in negative reviews.
    *   Calculated the L2 norm of year-over-year changes to identify keywords with significant shifts in frequency.
    *   Visualized keyword trends over time using line plots.
    *   Calculated Pointwise Mutual Information (PMI) scores between the most frequent nouns and adjectives in recent (2023-2024) negative reviews to identify statistically significant co-occurrences.
    *   Visualized PMI scores using a `seaborn` heatmap.
    *   Constructed and visualized a network graph (`networkx`) to illustrate strong associations (high absolute PMI) between nouns and adjectives.

7.  **Output Generation:**
    *   Saved the final populated SQLite database (`wwreviews.db`).
    *   Generated and saved visualizations (e.g., `pmi_heatmap.png`, `pmi_network.png`).
    *   Maintained a log of the scraping process (`crawling_logs.csv`).

## Key Skills & Tools Demonstrated

*   **Programming Language:** Python
*   **Data Manipulation & Analysis:** Pandas, NumPy
*   **Web Scraping:** `requests`, `BeautifulSoup4` (bs4)
*   **Database Management:** `sqlite3`
*   **Data Cleaning & Preprocessing:** Regular Expressions (re), String Manipulation
*   **Natural Language Processing (NLP):**
    *   `nltk`: Tokenization, POS Tagging, Lemmatization (WordNet), Sentiment Analysis (VADER)
    *   `spacy`: POS Tagging
    *   `textblob`: (Implicitly used or available if needed)
*   **Text Mining:** Keyword Extraction, Frequency Analysis, Word Clouds (`wordcloud`)
*   **Statistical Analysis:** Pointwise Mutual Information (PMI)
*   **Data Visualization:** `matplotlib`, `seaborn`, `wordcloud`, `networkx`
*   **Environment:** Google Colab / Jupyter Notebook

## Output Files

*   `wwreviews.db`: SQLite database containing `stores_list` and `store_reviews` tables.
*   `crawling_logs.csv`: Log file detailing the status (success, skipped, fail) of scraping for each store.
*   `pmi_heatmap.png`: Heatmap showing PMI values between top nouns and adjectives in recent bad reviews.
*   `pmi_network.png`: Network graph visualizing significant noun-adjective associations based on PMI.
*   *(Other plots generated within the notebook)*

## How to Use

1.  Clone the repository.
2.  Ensure Python and necessary libraries are installed (e.g., via `pip install -r requirements.txt` if provided, otherwise install: `pandas`, `requests`, `beautifulsoup4`, `nltk`, `spacy`, `matplotlib`, `seaborn`, `wordcloud`, `networkx`, `sqlalchemy`).
3.  Download required `nltk` data (e.g., `stopwords`, `punkt`, `averaged_perceptron_tagger`, `wordnet`, `vader_lexicon`) and the `spacy` English model (`python -m spacy download en_core_web_sm`).
4.  Prepare the `store_list.csv` file with store details and initial Google Review URLs.
5.  Run the scraping notebook/script first to populate the `wwreviews.db` database. Note: Scraping Google can be sensitive to IP blocks or changes in HTML structure.
6.  Run the analysis notebook sequentially to perform text preprocessing, sentiment analysis, trend analysis, PMI calculation, and generate visualizations.
7.  Review the generated database, CSV logs, and image files for results.
