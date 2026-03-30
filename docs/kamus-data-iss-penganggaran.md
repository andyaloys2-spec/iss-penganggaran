# Kamus Data ISS Penganggaran

## Entity: Anggaran
| Field Name          | Description                       | Data Type | Example       |
|---------------------|-----------------------------------|-----------|---------------|
| id_anggaran        | Unique identifier for anggaran    | Integer   | 1             |
| tahun              | Year the budget is allocated       | Integer   | 2026          |
| jumlah             | Total budget amount                | Decimal   | 50000.00      |

## Entity: Proyek
| Field Name          | Description                       | Data Type | Example       |
|---------------------|-----------------------------------|-----------|---------------|
| id_proyek          | Unique identifier for proyek       | Integer   | 101           |
| nama_proyek        | Name of the project                | String    | "Proyek A"   |
| anggaran_id        | Reference to the budget id         | Integer   | 1             |

## Entity: Pengeluaran
| Field Name          | Description                       | Data Type | Example       |
|---------------------|-----------------------------------|-----------|---------------|
| id_pengeluaran      | Unique identifier for pengeluaran   | Integer   | 1001          |
| proyek_id          | Reference to the project id        | Integer   | 101           |
| jumlah              | Amount spent                       | Decimal   | 15000.00      |