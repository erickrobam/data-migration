# data-migration

Python utility to read all records from a MySQL `product` table and export them to an Excel workbook.

## Source table

The exporter expects the source table created by the companion [product-database repository](https://github.com/erickrobam/product-database):

| Column | MySQL type | Excel column |
| --- | --- | --- |
| `product_id` | `BIGINT UNSIGNED` | Product ID |
| `name_product` | `VARCHAR(255)` | Product name |
| `date_product` | `DATE` | Product date |
| `value_product` | `DECIMAL(12,2)` | Product value |

The default connection database is `product_database`, and the table name is `product`.

## Requirements

- Python 3.10 or newer
- A running MySQL server containing the source table
- A MySQL account with `SELECT` permission on `product_database.product`

## 1. Install dependencies

Create and activate a virtual environment (recommended):

```bash
python -m venv .venv
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Windows PowerShell:

```powershell
.venv\\Scripts\\Activate.ps1
```

Install packages:

```bash
python -m pip install -r requirements.txt
```

## 2. Configure the MySQL connection

Copy `.env.example` to `.env` and edit the values:

```dotenv
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=your_mysql_username
MYSQL_PASSWORD=your_mysql_password
MYSQL_DATABASE=product_database
```

Use the hostname, port, username, password, and database name for the MySQL server where your product table actually lives. The `.env` file is ignored by Git; never commit real credentials.

## 3. Generate the Excel file

Run from the repository root:

```bash
python src/export_products.py
```

The script reads every row in `product`, ordered by `product_id`, and writes `products_export.xlsx` in the current directory. The workbook includes all four product fields, column filters, a frozen header row, date formatting, and numeric formatting for product values.

Choose another output path if needed:

```bash
python src/export_products.py --output reports/products.xlsx
```

If there are no rows, the script still creates a workbook with the column headers. The export is a snapshot of the data at the time the script runs; run it again to refresh the file.

## Project structure

```text
data-migration/
├── .env.example
├── .gitignore
├── README.md
├── requirements.txt
└── src/
    └── export_products.py
```
