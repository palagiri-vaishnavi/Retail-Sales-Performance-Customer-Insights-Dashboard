# Step 1 — Import Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Step 2 — Load Dataset
df = pd.read_csv('Sample - Superstore.csv', encoding='latin-1')
print("Shape:", df.shape)
print(df.head())

# Step 3 — Check Basic Info
print(df.info())
print(df.describe())

# Step 4 — Check Missing Values
print("Missing Values:\n", df.isnull().sum())

# Step 5 — Handle Missing Values
df['Postal Code'].fillna(df['Postal Code'].mode()[0], inplace=True)

# Step 6 — Remove Duplicates
print("Duplicates before:", df.duplicated().sum())
df.drop_duplicates(inplace=True)
print("Duplicates after:", df.duplicated().sum())

# Step 7 — Fix Data Types
df['Order Date'] = pd.to_datetime(df['Order Date'])
df['Ship Date'] = pd.to_datetime(df['Ship Date'])

# Step 8 — Drop Unnecessary Columns
df.drop(columns=['Row ID', 'Country'], inplace=True)

# Step 9 — Add Useful Columns
df['Order Year'] = df['Order Date'].dt.year
df['Order Month'] = df['Order Date'].dt.month
df['Order Month Name'] = df['Order Date'].dt.strftime('%B')
df['Shipping Days'] = (df['Ship Date'] - df['Order Date']).dt.days

# Step 10 — Clean Text Columns
df['Category'] = df['Category'].str.strip()
df['Segment'] = df['Segment'].str.strip()
df['Region'] = df['Region'].str.strip()

# Step 11 — Save Cleaned Data
df.to_csv('superstore_cleaned.csv', index=False)
print("✅ Cleaned data saved successfully!")
print("Final Shape:", df.shape)
