# CSV-from-Hell
A project designed to help practice and showcase my Python Data Cleaning Skills

Day 1: The "CSV from Hell" – Advanced Data Cleaning
Today’s goal is to move past df.dropna(). You’ll encounter a dataset that mirrors a real-world "legacy system export": inconsistent date formats, numbers disguised as strings with currency symbols, and "dirty" headers.

Project Introduction & Dataset Exploration (20 Minutes)
Dataset: Kaggle: Dirty Data Sample (or any raw sales CSV containing symbols like $, ,, and mixed date formats).

The Scenario: You’ve been handed a 5-year sales export. The previous analyst did everything in Excel, and the formatting is a nightmare. Before any analysis can happen, you must "type-safe" this data.

The Audit: Load the data and run df.info() and df.head(10).

Identify the Red Flags:

Columns that should be floats (e.g., Sales_Amount) are listed as object.

Dates like 01/02/2023 and 2023-02-01 are sitting in the same column.

Trailing spaces in categorical strings (e.g., "Victoria " vs "Victoria").

Hands-on Coding & Implementation (70 Minutes)
Phase 1: Header and String Sanitisation
The headers likely have spaces or weird casing. Clean them immediately to make your coding life easier.

Python
# Strip whitespace and replace spaces with underscores
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')
Phase 2: Numerical Rescue (Regex & Coercion)
You cannot perform math on $1,250.00. You need to strip non-numeric characters.

Task: Use .str.replace() with regex to remove currency symbols and commas.

The "Coerce" Trick: Use pd.to_numeric(df['column'], errors='coerce'). This turns unfixable garbage into NaN rather than crashing your script.

Phase 3: The Date Format War
In Australia, we use DD/MM/YYYY, but your data might be a mix of US and ISO formats.

Task: Use pd.to_datetime() with dayfirst=True and errors='coerce'.

Advanced: Identify rows that failed conversion and investigate why.

Phase 4: Deduplication with a Twist
Not all duplicates are identical rows. Sometimes a "duplicate" is a transaction with the same ID but a later timestamp.

Task: Use df.sort_values() followed by df.drop_duplicates(subset=['transaction_id'], keep='last').

Review, Debugging & Refinement (30 Minutes)
Debugging Exercise: "The Mystery of the Missing Rows"
The Problem: After running your cleaning script, you notice 5% of your sales data has vanished.
The Fix: Check your pd.to_numeric(errors='coerce') output.

Did you accidentally turn "0" into NaN?

Did a column have a placeholder like "N/A" that you didn't account for?

Deliverables
A Cleaned Script: A .py or .ipynb file that runs from top to bottom without errors.

The "Golden" File: Export the result using df.to_parquet('cleaned_sales.parquet'). (Parquet preserves the data types you worked so hard to fix, unlike CSV!).

Common Pitfalls to Watch For
The Regex Trap: Forgetting to escape the dollar sign \$ in your replace function (since $ is a special character in regex).

In-Place Modification: Avoid inplace=True. It’s being deprecated in many pandas functions. Instead, use df = df.method().

Case Sensitivity: "Melbourne" and "melbourne" are different to Python. Always use .str.lower() or .str.title() on categorical columns.
