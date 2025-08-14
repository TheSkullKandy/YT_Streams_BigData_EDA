# YouTube Trending Video Analytics with PySpark (Minimal)

This repository/notebook provides a clean, minimal PySpark workflow to analyze the **YouTube US Trending** dataset. It focuses on scalable data ingestion, cleaning, and exploratory analysis with interactive visualizations using Plotly. There are **no NLP/BERT components** in this version.

---

## Objectives

- Ingest the Kaggle US Trending dataset using PySpark DataFrames
- Clean and type-correct the data and map `category_id` to names
- Perform exploratory data analysis (EDA) on categories, channels, and engagement metrics
- Produce interactive visualizations and exportable artifacts

---

## Datasets

- `USvideos.csv` — per-video trending metadata (video_id, title, channel_title, category_id, views, likes, dislikes, comment_count, publish_time, tags, description)
- `US_category_id.json` — category mapping (id → name)

Download from Kaggle’s “YouTube Trending Video Dataset” and place the files as described below.

---

## Recommended Environment

- Python 3.9–3.11
- Java 8 or 11
- Apache Spark 3.4.x or 3.5.x recommended (works with earlier versions but may require dependency pins)
- Packages: `pyspark`, `pandas`, `plotly`

### Option A: Create a fresh environment (conda)

```bash
conda create -n yt-trending-basic python=3.11 -y
conda activate yt-trending-basic
pip install pyspark pandas plotly
```

### Option B: Use system Spark

Download a prebuilt Spark (example: Spark 3.5.0 on Hadoop 3):

```bash
curl -LO https://downloads.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz
tar xvf spark-3.5.0-bin-hadoop3.tgz
export SPARK_HOME="$PWD/spark-3.5.0-bin-hadoop3"
export PATH="$SPARK_HOME/bin:$PATH"
pip install pandas plotly
```

---

## Project Structure (suggested)

```
.
├── notebooks/
│   └── youtube_trending_analysis.ipynb      # Minimal PySpark analysis (no BERT)
├── data/
│   ├── USvideos.csv
│   └── US_category_id.json
└── outputs/
    ├── plot_top_categories_avg_views.html
    ├── plot_top_categories_avg_likes.html
    ├── plot_top_channels_trending.html
    └── plot_engagement_trends_over_time.html
```

You may choose a different layout; adjust paths inside the notebook accordingly.

---

## How to Run

1. Ensure the datasets are downloaded from Kaggle and placed under `./data` (or set absolute paths in the notebook).
2. Open `notebooks/youtube_trending_analysis.ipynb`.
3. Verify or set the two parameters/paths at the top of the notebook:
   - `CSV_PATH` → `data/USvideos.csv`
   - `CAT_PATH` → `data/US_category_id.json`
4. Run all cells.

The notebook will read the data, perform cleaning and EDA, and write interactive HTML plots to `./outputs/` (or the current working directory if paths are left as relative).

---

## Notebook Outline

1. **Setup**
   - Install required packages (if needed)
   - Create `SparkSession`

2. **Load Dataset**
   - Read `USvideos.csv` with robust CSV options (header, multiline, permissive mode)
   - Read and parse `US_category_id.json` into a Spark DataFrame

3. **Cleaning and Typing**
   - Normalize column names
   - Cast numeric columns (`views`, `likes`, `dislikes`, `comment_count`, `category_id`)
   - Parse dates (`trending_date`, `publish_time`)
   - Join category names; basic sanity checks; drop duplicates

4. **Exploratory Data Analysis**
   - **Top Categories** by average views and likes
   - **Top Channels** by number of trending appearances
   - **Engagement Over Time** (likes, dislikes, comments, views by date)

5. **Visualization**
   - Plotly bar and line charts
   - Export HTML plots for sharing

6. **Conclusion**
   - Inference and results summary

---

## Outputs

- `plot_top_categories_avg_views.html`
- `plot_top_categories_avg_likes.html`
- `plot_top_channels_trending.html`
- `plot_engagement_trends_over_time.html`

These files are written to the working directory or an `outputs/` folder if you configure one.

---

## Inference and Results (template)

- A small number of categories (often Music, Entertainment, Sports) dominate average views and frequency of trending appearances.
- A limited set of channels repeatedly trend, driven by large subscriber bases and consistent releases.
- Likes and views typically peak early; comment activity varies by category and tends to be higher for discussion-oriented content.

Replace the statements above with metrics extracted from your actual run for a data-backed report.

---

## Troubleshooting

- **Spark/Java not found**: Ensure Java 8/11 is installed and `JAVA_HOME` is set. If using system Spark, set `SPARK_HOME` and add it to `PATH`.
- **CSV parse issues**: The notebook uses `multiLine=True`, `quote='"'`, `escape='"'`, `mode='PERMISSIVE'`. Verify the file encoding (UTF-8).
- **Large collect to Pandas**: Only aggregated frames are converted. Avoid `.toPandas()` on large raw tables.
- **Dependency conflicts**: If a global package forces an incompatible version (e.g., `scikit-learn` vs. `tabpfn`), use a dedicated virtual environment.

---

## Extending This Notebook

- Add sentiment analysis, semantic embeddings, and clustering (BERT/Sentence-BERT)
- Compare with other countries (merge multiple Kaggle files)
- Build a dashboard (Dash/Streamlit) from the exported artifacts

---

## License and Data Use

This project is intended for educational and research purposes. Check Kaggle’s dataset licensing before redistribution.
