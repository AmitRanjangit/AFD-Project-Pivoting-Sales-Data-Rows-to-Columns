# AFD-Project-Pivoting-Sales-Data-Rows-to-Columns
# Azure Data Factory Project: Pivoting Sales Data (Rows to Columns)

This project demonstrates how to use Azure Data Factory (ADF) to transform Kaggle sales data by pivoting multiple product rows per transaction into a single wide row using Mapping Data Flows.

---

## Objective

Convert raw sales data where each product per order is stored as a row, into a pivoted format where each unique product becomes a column, with aggregated sales per transaction.

---

## Source Dataset

The dataset used is from Kaggle's Superstore Sales Data, which contains columns such as:

| Column Name     | Description                         |
|------------------|-------------------------------------|
| Row ID           | Unique row identifier               |
| Order ID         | ID of the customer's order          |
| Product Name     | Name of the product                 |
| Sales            | Sales amount of the item            |
| ...              | (Additional metadata columns)       |

Source: [Kaggle - Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

---

## Data Flow Structure (dataflow1)

The Mapping Data Flow has the following steps:

### Overall Flow: source1 → pivot1 → sink1

| Step      | Transformation | Description                                                                 |
|-----------|----------------|-----------------------------------------------------------------------------|
| source1   | Source          | Reads CSV file from Azure Blob (DelimitedText1).                           |
| pivot1    | Pivot           | Pivots Product Name values into columns, grouped by Row ID.                |
| sink1     | Sink            | Writes transformed output to new CSV in Blob (DelimitedText2).             |

---

## Pivot Transformation Details

| Setting                | Value                                     |
|------------------------|-------------------------------------------|
| Group By               | Row ID                                    |
| Pivot Key              | Product Name                              |
| Aggregation            | sales = sum(toInteger(Sales))             |
| Column Naming          | $N$V (ProductName becomes column)         |
| Column Layout          | Lateral                                   |

This transformation creates a wide table with one row per Row ID, and one column per product.

---

## Sample Transformation

### Input (Rows)

| Row ID | Product Name | Sales |
|--------|--------------|-------|
| 1      | Chair        | 250   |
| 1      | Phone        | 100   |
| 2      | Chair        | 300   |
| 2      | Monitor      | 200   |

### Output (Pivoted)

| Row ID | Chair | Phone | Monitor |
|--------|-------|-------|---------|
| 1      | 250   | 100   | null    |
| 2      | 300   | null  | 200     |

---

## Pipeline: SalesPivotPipeline

A pipeline was created with a Data Flow activity that executes the above flow:

| Component        | Description                                  |
|------------------|----------------------------------------------|
| Pipeline Name    | SalesPivotPipeline                           |
| Activity         | Executes dataflow1                           |
| Trigger          | Manual or scheduled                          |
| Input            | Blob container folder /input/               |
| Output           | Blob container folder /output/              |

---

## Directory Layout

ADF-Project-Pivoting-Sales-Data/
│
├── input/
│ └── sales_data.csv ← Raw Kaggle data
├── output/
│ └── pivoted_sales.csv ← Transformed data
├── dataflow/
│ └── dataflow1.json ← Full data flow definition
├── pipeline/
│ └── SalesPivotPipeline.json ← Pipeline configuration
└── README.md ← This file

yaml
Copy
Edit

---

## Concepts Demonstrated

| ADF Concept        | Description                                             |
|--------------------|---------------------------------------------------------|
| Mapping Data Flow  | Visual transformation engine for big data               |
| Pivot Transformation | Converts rows into columns dynamically                |
| Lateral Layout     | Uses dynamic sparse pivoting layout                     |
| GitHub Integration | ADF is connected to GitHub for version control          |
| Blob Storage       | Used for both source and sink data                      |
| Debugging          | Debug mode used for testing before deployment           |
