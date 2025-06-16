import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files
import io

print("=== PANDAS LIBRARY DEMONSTRATION ===")
print("This program demonstrates CSV file upload and data analysis using Pandas")
print("-" * 60)

# Step 1: Upload CSV file
print("Step 1: Uploading CSV file...")
print("Please select a CSV file to upload:")

uploaded = files.upload()


# Step 2: Process the uploaded file
print("\nStep 2: Processing uploaded file...")

filename = list(uploaded.keys())[0]
print(f"Uploaded file: {filename}")

# Step 3: Read CSV file into a Pandas DataFrame
print("\nStep 3: Reading CSV file into Pandas DataFrame...")

file_content = uploaded[filename]
df = pd.read_csv(io.StringIO(file_content.decode('utf-8')))

print("✓ CSV file successfully loaded into DataFrame!")
print(f"DataFrame shape: {df.shape}")

# Step 4: Display basic information about the dataset
print("\n" + "="*50)
print("DATASET OVERVIEW")
print("="*50)

# Display first few rows
print("\n1. First 5 rows of the dataset:")
print(df.head())


# Display basic information
print("\n2. Dataset Information:")
print(f"   - Number of rows: {len(df)}")
print(f"   - Number of columns: {len(df.columns)}")
print(f"   - Column names: {list(df.columns)}")


# Display data types
print("\n3. Data Types:")
print(df.dtypes)


# Display summary statistics for numeric columns
print("\n4. Summary Statistics (Numeric Columns):")
print(df.describe())


# Check for missing values
print("\n5. Missing Values Check:")
missing_values = df.isnull().sum()
print(missing_values)

if missing_values.sum() > 0:
    print(f"     Total missing values: {missing_values.sum()}")
else:
    print("    No missing values found!")


# Step 5: Advanced Data Analysis (if numeric columns exist)
numeric_columns = df.select_dtypes(include=[np.number]).columns

if len(numeric_columns) > 0:
    print("\n" + "="*50)
    print("ADVANCED ANALYSIS")
    print("="*50)


    print(f"\nNumeric columns found: {list(numeric_columns)}")

    # Calculate correlation matrix
    print("\n6. Correlation Matrix:")
    correlation_matrix = df[numeric_columns].corr()
    print(correlation_matrix)


    # Find columns with highest correlation
    if len(numeric_columns) > 1:
        # Get absolute values and remove diagonal
        corr_abs = correlation_matrix.abs()
        np.fill_diagonal(corr_abs.values, 0)

        # Find maximum correlation
        max_corr = corr_abs.max().max()
        max_corr_pair = corr_abs.stack().idxmax()

        print(f"\n7. Highest Correlation:")
        print(f"   Columns: {max_corr_pair[0]} & {max_corr_pair[1]}")
        print(f"   Correlation coefficient: {max_corr:.3f}")


else:
    print("\n  No numeric columns found for advanced analysis.")


# Step 6: Data Visualization (if possible)
print("\n" + "="*50)
print("DATA VISUALIZATION")
print("="*50)



if len(numeric_columns) > 0:
    # Create a simple histogram for the first numeric column
    plt.figure(figsize=(10, 6))


    first_numeric_col = numeric_columns[0]
    plt.hist(df[first_numeric_col].dropna(), bins=20, alpha=0.7, color='skyblue')
    plt.title(f'Distribution of {first_numeric_col}')
    plt.xlabel(first_numeric_col)
    plt.ylabel('Frequency')
    plt.grid(True, alpha=0.3)


    plt.tight_layout()
    plt.show()

    print(f"✓ Histogram created for column: {first_numeric_col}")
else:
    print("  No numeric columns available for visualization.")


# Step 7: Data Export Example
print("\n" + "="*50)
print("DATA EXPORT EXAMPLE")
print("="*50)

# Create a sample processed dataset
processed_df = df.copy()


# Add a sample calculated column if numeric data exists
if len(numeric_columns) > 0:
    first_col = numeric_columns[0]
    processed_df[f'{first_col}_normalized'] = (df[first_col] - df[first_col].mean()) / df[first_col].std()
    print(f"✓ Added normalized column for {first_col}")


# Show export options
print("\n8. Export Options Available:")
print("   - CSV: processed_df.to_csv('output.csv', index=False)")
print("   - Excel: processed_df.to_excel('output.xlsx', index=False)")
print("   - JSON: processed_df.to_json('output.json')")


# Step 8: Summary and Conclusion
print("\n" + "="*60)
print("ANALYSIS COMPLETE - SUMMARY")
print("="*60)


print(f"""
 Successfully processed CSV file: {filename}
 Dataset contains {len(df)} rows and {len(df.columns)} columns
 Numeric columns: {len(numeric_columns)}
 Missing values: {df.isnull().sum().sum()}
 Data types: {df.dtypes.value_counts().to_dict()}
""")


print(" Pandas demonstration completed successfully!")
print("\nThis program demonstrated:")
print("• File upload capabilities")
print("• Data loading and inspection")
print("• Statistical analysis")
print("• Data visualization")
print("• Export options")
