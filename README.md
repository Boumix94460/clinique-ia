# clinique-ia

## Running the notebook

1. Clone the repository and open `edtech_ai_clinic.ipynb` in Jupyter or VS Code.
2. Install the dependencies in the Python environment used by the notebook:

   ```bash
   python -m pip install pandas kagglehub
   ```

3. Run the notebook cells. The data-loading cell automatically downloads
   version 45 of the public
   [Education Statistics](https://www.kaggle.com/datasets/theworldbank/education-statistics)
   dataset from Kaggle the first time it runs. KaggleHub caches it locally, so
   the large files do not need to be stored on GitHub or downloaded again on
   every run. An internet connection is required for the initial download.
