# Health-wise Explainable Movie Recommender System (HEMRS)

## Overview

This repository contains the source code, datasets, and supplementary materials for the **Health-wise Explainable Movie Recommender System (HEMRS)**, a research project developed as part of a thesis to enhance movie recommendations by integrating collaborative filtering, content-based filtering, sentiment analysis, and explainable AI. HEMRS prioritizes health-aligned recommendations (e.g., uplifting or calming movies to improve mood/stress) and provides interpretable explanations using LIME, addressing challenges like the cold start problem, data sparsity, and user trust (Schedl et al., 2018; Ribeiro et al., 2016). The system was evaluated using the MovieLens 1M and IMDB datasets, achieving an RMSE of 0.72 and 85% user satisfaction for explainability, as detailed in the thesis (Chapter 4). This repository is intended for researchers and developers interested in reproducing the results or extending the work.

## Prerequisites

To run the HEMRS code, ensure you have the following installed:
- **Python**: Version 3.8 or higher
- **Dependencies**: Listed in `requirements.txt`
  - `numpy`, `pandas`, `scikit-learn` (for matrix factorization and content-based filtering)
  - `vaderSentiment` (for sentiment analysis)
  - `lime` (for explainable AI)
  - `matplotlib`, `seaborn` (for visualizations)
- **Hardware**: Minimum 8GB RAM, 4GB disk space for datasets
- **Optional**: Jupyter Notebook for interactive exploration

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/HEMRS-Research/HEMRS.git
   cd HEMRS
   ```

2. **Set Up a Virtual Environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Download Datasets**:
   - The preprocessed MovieLens 1M and IMDB datasets are included in the `data/` directory. If you need to download raw datasets:
     - MovieLens 1M: Available at [grouplens.org](https://grouplens.org/datasets/movielens/1m/) (Harper & Konstan, 2015)
     - IMDB: Use the provided `data/imdb_scraper.py` to scrape reviews (requires an API key; see `data/README.md` for instructions).

## Usage

The repository is organized to allow easy execution of the HEMRS pipeline, from data preprocessing to recommendation generation and evaluation.

1. **Preprocessing**:
   - Run `preprocess_data.py` to clean and format MovieLens 1M and IMDB datasets:
     ```bash
     python scripts/preprocess_data.py
     ```
   - Outputs are saved in `data/processed/` as `movielens_ratings.csv` and `imdb_reviews.csv`.

2. **Running HEMRS**:
   - Execute the main script to train and generate recommendations:
     ```bash
     python scripts/run_hemrs.py --dataset movielens --output results/recommendations.csv
     ```
   - Options:
     - `--dataset`: Choose `movielens` or `imdb` (default: `movielens`)
     - `--output`: Specify output file for recommendations
     - `--explain`: Enable LIME explanations (default: False)

3. **Evaluating Results**:
   - Compute RMSE, precision, and recall using:
     ```bash
     python scripts/evaluate.py --results results/recommendations.csv
     ```
   - Visualizations (e.g., RMSE plots, user study charts) are generated in `results/plots/`.

4. **User Study Analysis**:
   - Analyze user study responses (from Appendix A.4) using:
     ```bash
     python scripts/analyze_user_study.py --data user_study_data/responses.csv
     ```
   - Outputs summary statistics and charts in `user_study_data/outputs/`.

5. **Example Notebook**:
   - Explore the pipeline interactively with `notebooks/hemrs_demo.ipynb`, which includes examples of recommendation generation and LIME explanations.

## Directory Structure

```plaintext
HEMRS/
├── data/
│   ├── raw/                    # Raw MovieLens 1M and IMDB datasets
│   ├── processed/              # Preprocessed datasets (CSV format)
│   ├── imdb_scraper.py         # Script for scraping IMDB reviews
│   └── README.md               # Dataset-specific instructions
├── scripts/
│   ├── preprocess_data.py      # Data cleaning and formatting
│   ├── matrix_factorization.py  # Collaborative filtering implementation
│   ├── content_based_filtering.py # TF-IDF and Doc2Vec implementation
│   ├── sentiment_analysis.py   # VADER sentiment analysis
│   ├── lime_explainability.py  # LIME explanation generation
│   ├── run_hemrs.py           # Main script for running HEMRS
│   ├── evaluate.py            # Evaluation metrics (RMSE, precision, recall)
│   └── analyze_user_study.py   # User study analysis
├── notebooks/
│   └── hemrs_demo.ipynb       # Interactive demo notebook
├── user_study_data/
│   ├── responses.csv          # Anonymized user study responses
│   └── outputs/               # Analysis results (charts, statistics)
├── results/
│   ├── recommendations.csv     # Generated recommendations
│   └── plots/                 # Evaluation and user study visualizations
├── requirements.txt           # Python dependencies
└── README.md                  # This file
```

## Datasets

- **MovieLens 1M**:
  - Source: [GroupLens](https://grouplens.org/datasets/movielens/1m/)
  - Description: 1 million ratings from 6,040 users for 3,952 movies, with metadata (genres, release years, user demographics). Used for collaborative filtering and evaluation (Harper & Konstan, 2015).
  - Limitations: Overrepresentation of popular genres (e.g., drama, comedy); lacks review text for sentiment analysis.
  - Location: `data/raw/movielens/` and `data/processed/movielens_ratings.csv`

- **IMDB**:
  - Source: Scraped via IMDB API (see `data/imdb_scraper.py`)
  - Description: Movie metadata (genres, plots, cast) and user reviews for ~50,000 movies, used for sentiment analysis with VADER.
  - Limitations: English-language bias, potential noise in reviews (e.g., sarcasm).
  - Location: `data/raw/imdb/` and `data/processed/imdb_reviews.csv`

## User Study Data

The user study involved 50 participants evaluating HEMRS against a baseline system (Appendix A.4). Key results:
- **Health Alignment**: Mean scores of 4.12 (mood/stress improvement) and 4.24 (well-being alignment), with 82% agreement (scores 4 or 5) for HEMRS vs. 3.45 (60% agreement) for the baseline.
- **Explainability**: Mean scores of 4.30 (clarity) and 4.25 (trust), with 85% agreement for LIME-based explanations (Ribeiro et al., 2016).
- **Satisfaction**: Mean score of 4.15, with 80% satisfied with HEMRS vs. 65% for the baseline.
- Anonymized responses and analysis scripts are in `user_study_data/`.

## Contributing

Contributions are welcome! Please:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/new-feature`).
3. Commit changes (`git commit -m "Add new feature"`).
4. Push to the branch (`git push origin feature/new-feature`).
5. Open a pull request.

Report issues or suggestions via the [Issues](https://github.com/HEMRS-Research/HEMRS/issues) tab.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details. Users are free to use, modify, and distribute the code and datasets with proper attribution.

## References

- Harper, F. M., & Konstan, J. A. (2015). The MovieLens Datasets: History and Context. *ACM Transactions on Interactive Intelligent Systems*, 5(4), 1–19. https://doi.org/10.1145/2827872
- Schedl, M., Flexer, A., & Urbano, J. (2018). Emotion-aware Music Recommendation. *Proceedings of the ACM Conference on Recommender Systems*, 3240323. https://dl.acm.org/doi/10.1145/3240323.3240344
- Ribeiro, M. T., Saadi, D., & Guestrin, C. (2016). Why Should I Trust You? Explaining the Predictions of Any Classifier. *arXiv preprint arXiv:1602.04938*. https://arxiv.org/abs/1602.04938

## Contact

For questions or support, contact the research team via [GitHub Issues](https://github.com/HEMRS-Research/HEMRS/issues) or email (hemrs.research@example.com).
