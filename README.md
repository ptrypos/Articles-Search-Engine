# Articles Search Engine

A desktop news search application built with Java Swing and Apache Lucene. The app indexes a CSV of CNN articles and lets you search by keywords, author, category, or headline with pagination, sorting, and result highlighting.

## Project structure
- `src/mye003/searchenginenews/` — GUI, controllers, services, and DTOs for the search experience.
- `resources/` — `CNN_Articles_clean.csv` dataset, Lucene index output directory (`resources/index`), and the application logo.
- `lib/` — Third-party dependencies (Apache Lucene 10.x modules, OpenCSV, Apache Commons Lang).
- `bin/` — Compilation output directory when building locally.

## Prerequisites
- Java 17 or later (the project was developed against JDK 21).
- No external services are required; all dependencies are vendored in `lib/`.

## Building
1. From the repository root, compile the sources into `bin/`:

   ```bash
   javac \
     -d bin \
     -cp "lib/commons-lang3-3.17.0.jar:lib/opencsv-5.10.jar:lib/lucene_modules/*" \
     $(find src -name "*.java")
   ```

   On Windows, replace the classpath separators (`:`) with semicolons (`;`).

2. Ensure the `resources/` directory remains alongside the compiled classes so the CSV and index folder are accessible at runtime.

## Running
After compiling, launch the Swing GUI from the project root:

```bash
java -cp "bin:lib/commons-lang3-3.17.0.jar:lib/opencsv-5.10.jar:lib/lucene_modules/*" mye003.searchenginenews.SearchEngineNewsGUI
```

The application will build a Lucene index from `resources/CNN_Articles_clean.csv` on first run (or if the `resources/index` directory is missing). Subsequent launches reuse the existing index for faster startup.

## Features
- **Multi-field search:** Choose keywords, author, category, or article title (second headline) as the search field.
- **Relevance tuning:** Lucene-based TF-IDF scoring with lightweight boosting informed by recent search history.
- **Result presentation:** Clickable article titles, snippets with highlighted query terms, published date display, and alphabetical sorting option.
- **Pagination:** Ten results per page with quick navigation controls.

## Troubleshooting
- If indexing fails, delete the `resources/index` directory and restart the app to trigger a rebuild.
- Ensure your working directory is the repository root when running so relative paths to `resources/` resolve correctly.

## 👨‍💻 Author

**Panagiotis Trypos**

* [Website / Portfolio](https://ptrypos.me)
* [GitHub Profile](https://github.com/ptrypos)
* [LinkedIn](https://www.linkedin.com/in/%CF%80%CE%B1%CE%BD%CE%B1%CE%B3%CE%B9%CF%8E%CF%84%CE%B7%CF%82-%CF%84%CF%81%CF%8D%CF%80%CE%BF%CF%82-983abb242)
