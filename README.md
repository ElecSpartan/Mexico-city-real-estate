# Mexico City Real Estate - EDA and Wrangling

## Project Overview
This project performs an Exploratory Data Analysis (EDA) and data cleaning pipeline on a set of Mexico City real estate data. The notebook loads multiple CSV files, cleans them, explores distributions, and handles outliers.  

The primary goal is to create a reproducible data "wrangle" function that can be applied to any of the raw data files to prepare them for analysis or machine learning. The notebook concludes with a main execution block (`main()`) that allows a user to interactively select a data file (1-5 or a combined file) to process through this cleaning pipeline.

---

## Key Analysis & Cleaning Steps

### Data Combination
- Uses `glob` to find all `mexico-city-real-estate-*.csv` files in the `data/` directory.
- Concatenates them into a single DataFrame.

### Outlier Removal
- Uses the Interquartile Range (IQR) method to identify and remove outliers from key numerical columns:
  - `surface_covered_in_m2`
  - `price_aprox_usd`

### Feature Engineering
- Splits the `lat-lon` column into two separate numeric columns: `lat` and `lon`.
- Extracts the borough (e.g., "Miguel Hidalgo", "Tlalpan") from the `place_with_parent_names` column.

### Column Pruning
- **High-Nulls:** Drops columns with more than 50% missing values (e.g., `expenses`, `floor`, `rooms`).
- **Low/High Cardinality:** Drops columns with only one unique value (`operation`) or very high, non-useful cardinality (`properati_url`).
- **Multicollinearity:** Drops redundant price columns (`price`, `currency`, `price_aprox_local_currency`, `price_per_m2`) to keep `price_aprox_usd` as the single price feature.

### Exploratory Analysis (EDA)
- Plots the distribution of apartment sizes (`surface_covered_in_m2`) before and after outlier removal using histograms and box plots.
- Generates a bar plot of the mean price by borough.
- Creates scatter plots to visualize the relationship between apartment size and price, showing a moderate positive correlation after cleaning.

---

## Main Functions

### Data Cleaning
- `wrangler(file_path)`: Main function that encapsulates the entire data loading and cleaning pipeline. Returns a cleaned DataFrame.
- `remove_outliers_iqr(df, columns)`: Helper function to remove outliers from specified DataFrame columns using the 1.5 * IQR rule.

### Visualization Helpers
- `Line_Plot(...)`
- `Bar_Plot(...)`
- `Pie_Chart(...)`
- `Histogram_Chart(...)`
- `Scatter_Plot(...)`

---

## How to Use

### File Structure
1. Place this Jupyter Notebook in your project's root directory.
2. Create a subdirectory named `data/`.
3. Place all the `mexico-city-real-estate-*.csv` files inside the `data/` directory.

### Run the Notebook
1. Execute the cells in order.
2. The notebook will combine all CSVs into `data/mexico-city-real-estate-combined.csv`.
3. The final cell (`if __name__ == "__main__":`) will start an interactive prompt.

### Interactive Prompt
- You will be asked:  
  `Enter the file number or combined to wrangle (1, 2, 3, 4, 5, or combined) or 'q to quit':`
- Enter a number (e.g., `1`) to wrangle just that file, or type `combined` to wrangle the complete dataset.
- The notebook will then load, clean, and display the `head()` and `info()` of the wrangled DataFrame.
- Enter `q` to quit the loop.

---

## Dependencies
The notebook requires the following Python libraries:
- `pandas`
- `numpy`
- `matplotlib`
- `glob`
