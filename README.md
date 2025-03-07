# Transaction Analysis with OpenAI

## 📖 Overview
This project utilizes **OpenAI's GPT-4 Turbo** to extract and process user queries related to financial transactions. It corrects spelling, normalizes spending categories, extracts SQL parameters, and queries an SQLite database to retrieve transaction summaries.

## Features
- **Natural Language Query Processing**: Understands user queries about transactions.
- **Spelling & Grammar Correction**: Fixes errors in user input.
- **Category Normalization**: Maps queries to predefined spending categories.
- **SQL Query Generation**: Extracts relevant SQL parameters.
- **Database Query Execution**: Fetches results from an SQLite database.
- **Performance Timing**: Tracks execution time for each step.

## 🔧 Setup & Installation

## 🚀 Installation Guide

### 1. Clone the Repository
```bash
git clone https://github.com/victormelatti/customer_insight.git
```

### 2. Create a virtual environenment using the .yml file (the env name name is in the file )
```bash
conda conda env create -f environment.yml
conda activate AI_Spending_Insights
```

### if conda is not available
```bash
python -m venv AI_Spending_Insights
```
#### Activate env and install requirements

activate the env, then install requirements:
```bash
pip install -r requirements.txt
``` 

### **2️⃣ Set OpenAI API Key** 
Open the notebook and update the API key in the script (second cell). For security reasons, github does not allow pushing secret OpenAI keys. The API key is provided in the email from victor.melatti01@gmail.com

```python
os.environ['OPENAI_API_KEY'] = 'your-api-key'
```

### **3️⃣ Run the Notebook**
Run all cells 

The process is detailed in the notebook and below:

## 📊 SQLite Database Schema
The script initializes a database with two tables:

### **1️⃣ Transactions Table** (`transactions`)
| Column          | Type  | Description  |
|---------------|------|--------------|
| transaction_id | TEXT  | Unique ID for each transaction |
| date          | TEXT  | Transaction date (YYYY-MM-DD) |
| amount        | REAL  | Transaction amount |
| merchant_id   | TEXT  | Foreign key linking to `def_merchants` |

### **2️⃣ Merchant Definition Table** (`def_merchants`)
| Column        | Type  | Description |
|--------------|------|-------------|
| merchant_id  | TEXT | Unique merchant identifier |
| merchant     | TEXT | Name of the merchant |
| spending_cat | TEXT | Spending category (e.g., "GROCERIES") |

## 📌 Functionality Breakdown
### **1️⃣ Query Preprocessing** (`preprocess_user_query`)
- Fixes spelling errors
- Normalizes spending categories

### **2️⃣ SQL Query Generation** (`generate_sql_query`)
- Converts user query into SQL syntax
- Filters by date, merchant, and spending category

### **3️⃣ Query Execution** (`call_database`)
- Runs the generated SQL query on the SQLite database

### **4️⃣ Full Pipeline Execution** (`function_calling_db`)
- Calls **OpenAI/Gemini** to extract parameters
- Generates SQL query
- Executes query and returns results
- logs errors if found

## 📊 Example Queries & Results
The script runs the following queries:
```python
user_queries = [
    "How much did I spend on groceries in the last 4 months?",
    "What was the total spend at McDonald’s last week?",
    "What was the max I have spent on groceries in the last year?",
    "How many times did I go to the cinema in the last 3 months?",
    "How much did I invest in Starling since January 1st, 2025?"
]
```
## Validation of test set
A benchmark file is provided to measure the accuracy of the model. The benchmar file contains the correct answers that the process should return. 
A PASSED TEST? column is created showing True/False for each test and a % of success is computed


### **Sample Output (Pandas DataFrame)**
| Original Query  | Cleaned Query | Start Date | End Date | Merchant | Spending Category | Response | Total Time (s) | errors
|----------------|--------------|------------|----------|----------|-------------------|----------|----------------|
| How much did I spend on groceries in the last 4 months? | How much did I spend in GROCERIES in the last 4 months? | 2024-11-01 | 2025-03-04 | NULL | GROCERIES | 345.75 | 0.2156 | []

## 📌 Notes
- The script defaults to **GPT-4 Turbo**
- The SQLite database is **recreated each time** the script runs.
- The **first available transaction date** in the database is dynamically set as the fallback `start_date`.

## 📌 Future Improvements
The notebook contains notes regarding future work on 
- Fast Inference 
- Scalability 


