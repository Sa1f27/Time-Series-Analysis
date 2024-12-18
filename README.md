# Data Processing Function Documentation

## Overview
This script contains a Python function, `process_data`, designed to preprocess and clean sales data by integrating additional datasets such as oil prices and holiday information. It prepares the data for machine learning tasks by performing tasks like merging datasets, handling missing values, and encoding categorical features.

---

## Function Description: `process_data`

### Purpose
The `process_data` function preprocesses the input DataFrame and enhances it with external data sources to create a ready-to-use dataset for training and testing machine learning models.

### Key Features
- **Dataset Integration**: Merges the input dataset with oil price and holiday data.
- **Missing Data Handling**: Fills missing values for holiday types, crude oil prices, and other columns.
- **Feature Engineering**: Creates additional features such as `is_holiday`, `day_number`, and one-hot encoding for product families.
- **Robust Error Handling**: Includes safeguards to identify missing columns or merging issues.

---

### Function Signature
```python
process_data(df: pd.DataFrame, is_test: bool = False) -> pd.DataFrame
```

#### Parameters
- **`df`** (`pd.DataFrame`): The input DataFrame containing sales data.
- **`is_test`** (`bool`, default=`False`): A flag to indicate whether the dataset is for training or testing. Determines which columns are expected in the input.

#### Returns
- **`pd.DataFrame`**: A cleaned and enriched DataFrame ready for machine learning.

---

## Detailed Processing Steps

### 1. Column Selection
Depending on whether the input is training or testing data:
- **Training Data**: Requires columns like `'date'`, `'family'`, `'onpromotion'`, `'sales'`, `'store_nbr'`, and `'id'`.
- **Test Data**: Requires columns like `'date'`, `'family'`, `'onpromotion'`, `'store_nbr'`, and `'id'`.

### 2. Dataset Merging
- **Oil Data**: Merged on the `'date'` column to add crude oil prices (`dcoilwtico`).
- **Holiday Data**: Merged on the `'date'` column to add holiday types and transfer status.

### 3. Feature Engineering
- **`is_holiday`**: Encodes holiday information based on the type of day and whether it is transferred.
- **`onpromotion`**: Converts promotional status into binary values (0 or 1).
- **`day_number`**: Encodes the date as an integer for machine learning models.
- **`family` Encoding**: Performs one-hot encoding for the `family` column to represent product categories as binary features.

### 4. Handling Missing Data
- **Crude Price**: Forward fills missing values.
- **Other Columns**: Removes rows with missing values after processing.

---

## Requirements

### Input Data
- **Sales Data (`df`)**: Must contain the required columns for training or testing as specified.
- **Oil Data (`oil_data`)**: Must include `date` and `dcoilwtico` (crude price).
- **Holiday Data (`holidays_data`)**: Must include `date`, `type`, and `transferred`.

### External Libraries
- **Pandas**: For data manipulation.
- **Numpy**: For numerical operations.

Install these libraries using:
```bash
pip install pandas numpy
```

---

## Example Usage
```python
import pandas as pd

# Load datasets
sales_data = pd.read_csv("sales.csv")
oil_data = pd.read_csv("oil.csv")
holidays_data = pd.read_csv("holidays.csv")

# Process training and test datasets
train_data = process_data(sales_data)
test_data = process_data(sales_data, is_test=True)

print(train_data.head())
```

---

## Error Handling
### Common Errors
- **`KeyError: '[...] not in index'`**
  - Cause: Missing required columns in the input DataFrame or external datasets.
  - Solution: Verify input data using `print(df.columns)` before calling the function.

- **`MergeError`**
  - Cause: Inconsistent keys between datasets (e.g., missing `date` in `oil_data` or `holidays_data`).
  - Solution: Check the `date` column in all datasets for consistency.

---

## Limitations
- Assumes all input data is clean and free of duplicates.
- Requires external datasets (`oil_data`, `holidays_data`) to be pre-loaded.
- Drops rows with missing values after processing, which might affect the dataset size.

---

## Future Enhancements
- Add logging for each processing step.
- Provide more granular error handling and fallback options.
- Allow flexibility in column names through configuration parameters.

---

## License
This script is open-source and available for educational and non-commercial use.

---

## Author
**Mohammed Huzaifah**
- Aspiring Machine Learning Engineer
- Specialization in AI and Machine Learning

