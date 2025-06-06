# AFD-Project-Pivoting-Sales-Data-Rows-to-Columns
This Azure Data Factory project pivots sales data by transforming product entries from rows into columns, grouped by month. It uses ADF's mapping data flow to convert row-based sales records into a columnar format for easy analysis. Input is from CSV; output goes to blob or SQL.
This Azure Data Factory project pivots sales data by transforming product entries from rows into columns, grouped by month. It uses ADF's mapping data flow to convert row-based sales records into a columnar format for easy analysis. Input is from CSV; output goes to blob or SQL.

---

## Tools Used
- Azure Data Factory (Mapping Data Flow)
- Azure Blob Storage (input/output CSV)
- Sample datasets from Kaggle or GitHub

---

## Sample Input

| Month | Product | Sales |
|-------|---------|-------|
| Jan   | A       | 100   |
| Jan   | B       | 90    |
| Feb   | A       | 120   |
| Feb   | B       | 110   |

---

## 📈 Output After Pivot

| Month | A   | B   |
|-------|-----|-----|
| Jan   | 100 | 90  |
| Feb   | 120 | 110 |

---

## ⚙️ Data Flow Logic
- **Group by**: Month  
- **Pivot key**: Product  
- **Pivoted column**: Sales
