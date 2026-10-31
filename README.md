# Articles Search Engine

A desktop search engine for news articles, built with **Java Swing** and **Apache Lucene 10**. It indexes CNN news articles from a CSV dataset and lets you search them by keywords, author, category or headline, with pagination, sorting and query-term highlighting. Search results are also re-ranked using the user's recent search history.

Developed for the **Information Retrieval (MYE003)** course at the Department of Computer Science & Engineering, University of Ioannina (2025).

<img width="1919" height="1019" alt="Screenshot_1" src="https://github.com/user-attachments/assets/ec60eb5f-b01a-462a-8911-5c87a249aa80" />


## Features
- **Multi-field search:** search by keywords, author, category, or article title (second headline).
- **Full Lucene query syntax:** boolean operators, phrases, wildcards, etc., via Lucene's `QueryParser`.
- **History-based re-ranking:** terms that appear often in the top results of your recent searches get a small boost in new searches.
- **Result presentation:** clickable article titles, snippets with highlighted query terms, and publication dates.
- **Sorting:** by relevance (default) or alphabetically by title.
- **Pagination:** 10 results per page.

## How it works

### Indexing
On first launch, the app reads `resources/CNN_Articles_clean.csv` with OpenCSV and turns each row into a Lucene Document with 11 fields (author, date, category, section, URL, headline, description, keywords, second headline, article text, etc.), analyzed with Lucene's `StandardAnalyzer`. Malformed rows are skipped. The index is stored in `resources/index/` and reused on later launches.

### Searching
The user's query is parsed with `QueryParser` on the selected field and scored with Lucene's default ranking function (**BM25**).

### History-based boosting
The app keeps the **last 20 searches**, together with the **top 5 documents** each one returned. For every new search:
1. It counts how often each term (3+ characters) appears in the selected field of those stored documents (**TF**).
2. It weights each term by its inverse document frequency in the whole index: `idf = log((N + 1) / (df + 1))`.
3. Terms with a score `tf × idf × 0.3` above 1.0 are candidates, and the **top 10** are added to the query as optional (`SHOULD`) clauses with that score as their boost.

The original query stays mandatory (`MUST`), so the boosted terms only change the **ranking** of matching articles, not which articles match. In effect, results related to what the user searched for recently are moved higher.

## Project structure
```
src/mye003/searchenginenews/
├── SearchEngineNewsGUI.java     # Swing GUI (home screen, results, pagination)
├── controllers/
│   ├── IndexController.java     # Builds the index on startup if missing
│   └── SearchController.java    # Runs searches, caches and paginates results
├── services/
│   ├── IndexService.java        # CSV → Lucene index
│   └── SearchService.java       # Query parsing, history-based boosting, search
└── dto/
    └── SearchResult.java        # Title, URL, description, date, score
resources/                       # Dataset, logo, and generated index/
lib/                             # Dependencies (Lucene 10.1, OpenCSV 5.10, Commons Lang 3.17)
bin/                             # Compiled classes
```

## Dataset
CNN news articles in CSV form (`resources/CNN_Articles_clean.csv`), with columns: Index, Author, Date published, Category, Section, URL, Headline, Description, Keywords, Second headline, Article text.

Source: [CNN Articles after basic cleaning (Kaggle)](https://www.kaggle.com/datasets/hadasu92/cnn-articles-after-basic-cleaning)

## Requirements
- Java 17 or later (developed with JDK 21).
- No external services; all dependencies are included in `lib/`.

## Build & Run
From the repository root:

```bash
# Compile
javac -d bin \
  -cp "lib/commons-lang3-3.17.0.jar:lib/opencsv-5.10.jar:lib/lucene_modules/*" \
  $(find src -name "*.java")

# Run
java -cp "bin:lib/commons-lang3-3.17.0.jar:lib/opencsv-5.10.jar:lib/lucene_modules/*" \
  mye003.searchenginenews.SearchEngineNewsGUI
```

On Windows, use `;` instead of `:` as the classpath separator.

Run the app from the repository root so the relative paths to `resources/` resolve correctly. The first launch builds the index, which takes a little longer; later launches reuse it.

Alternatively, import the project into **Eclipse** (the `.project` / `.classpath` files are included) and run `SearchEngineNewsGUI`.

## Troubleshooting
- **Indexing fails or results look wrong:** delete `resources/index/` and restart the app to rebuild the index.
- **Dataset or logo not found:** make sure the working directory is the repository root.

## Authors

**Panagiotis Trypos**
- [Website / Portfolio](https://ptrypos.me)
- [GitHub](https://github.com/ptrypos)
- [LinkedIn](https://www.linkedin.com/in/%CF%80%CE%B1%CE%BD%CE%B1%CE%B3%CE%B9%CF%8E%CF%84%CE%B7%CF%82-%CF%84%CF%81%CF%8D%CF%80%CE%BF%CF%82-983abb242)

**Thanos Tonios**
- [GitHub](https://github.com/athanasios-tonios)
