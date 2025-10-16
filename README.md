# Credit Card Weekly Dashboard

## Project Overview

A comprehensive credit card analytics dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

## Table of Contents

- [Features](#features)
- [Data Setup](#data-setup)
- [DAX Measures](#dax-measures)
- [Key Insights](#key-insights)
- [Requirements](#requirements)
- [Installation](#installation)

## Features

- **Real-time Performance Monitoring**: Track weekly credit card operations
- **Customer Segmentation**: Analyze by age groups and income levels
- **Revenue Analysis**: Comprehensive revenue tracking with week-over-week comparisons
- **Transaction Insights**: Monitor transaction amounts and counts
- **Geographic Analysis**: State-wise performance tracking
- **Card Type Performance**: Analysis across different credit card categories

## Data Setup

### 1. Prepare CSV Files

Ensure you have the following CSV files ready:
- `cust_detail.csv` - Customer demographic information
- `cc_detail.csv` - Credit card transaction details

### 2. Create SQL Tables

```sql
-- Create customer details table
CREATE TABLE cust_detail (
    customer_id INT PRIMARY KEY,
    customer_age INT,
    income DECIMAL(10,2),
    -- Add other relevant columns
);

-- Create credit card details table
CREATE TABLE cc_detail (
    transaction_id INT PRIMARY KEY,
    customer_id INT,
    week_start_date DATE,
    annual_fees DECIMAL(10,2),
    total_trans_amt DECIMAL(10,2),
    interest_earned DECIMAL(10,2),
    -- Add other relevant columns
    FOREIGN KEY (customer_id) REFERENCES cust_detail(customer_id)
);
```

### 3. Import CSV Files

Import the prepared CSV files into the SQL database using your preferred method (SQL IMPORT, BULK INSERT, or database management tools).

## DAX Measures

### Customer Segmentation

#### Age Group Classification
```dax
AgeGroup = SWITCH(
    TRUE(),  
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)
```

#### Income Group Classification
```dax
IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)
```

### Time Intelligence

#### Week Number Calculation
```dax
week_num2 = WEEKNUM('public cc_detail'[week_start_date])
```

### Revenue Metrics

#### Total Revenue
```dax
Revenue = 'public cc_detail'[annual_fees] + 
          'public cc_detail'[total_trans_amt] + 
          'public cc_detail'[interest_earned]
```

#### Current Week Revenue
```dax
Current_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])
    )
)
```

#### Previous Week Revenue
```dax
Previous_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(
        ALL('public cc_detail'),
        'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) - 1
    )
)
```

## Key Insights

### Week 53 Performance (31st December)

#### Week-over-Week Changes
- **Revenue Growth**: +28.8%
- **Transaction Amount**: Increased
- **Transaction Count**: Increased
- **Customer Count**: Growing

#### Year-to-Date Overview
- **Total Revenue**: $57M
- **Total Interest**: $8M
- **Total Transaction Amount**: $46M

#### Customer Demographics
- **Male Customers**: $31M revenue contribution
- **Female Customers**: $26M revenue contribution

#### Card Performance
- **Blue & Silver Cards**: 93% of overall transactions
- Top performing card categories driving business

#### Geographic Distribution
- **Top States**: TX, NY, CA
- **Combined Contribution**: 68% of total revenue

#### Credit Metrics
- **Overall Activation Rate**: 57.5%
- **Overall Delinquent Rate**: 6.06%

## Requirements

- SQL Database (PostgreSQL, MySQL, or SQL Server)
- Power BI Desktop
- CSV data files for customer and credit card details

## Installation

1. Set up your SQL database
2. Create the required tables using the provided schema
3. Import CSV files into the database
4. Open Power BI Desktop
5. Connect to your SQL database
6. Import the data tables
7. Apply the DAX measures provided above
8. Build visualizations based on the key metrics

## Dashboard Components

- Revenue trends and WoW comparison
- Customer segmentation analysis
- Geographic heat maps
- Card type performance breakdown
- Activation and delinquency rate monitoring
- Transaction volume and amount tracking

## Usage

1. Refresh data weekly to update dashboard metrics
2. Monitor WoW changes for quick performance assessment
3. Drill down into specific segments for detailed analysis
4. Use filters to analyze by card type, geography, or customer segment

## Contributing

For questions or contributions, please contact the project stakeholders.

## License

Internal use only - Confidential business intelligence dashboard

---

**Last Updated**: Week 53, 2024  
**Dashboard Version**: 1.0
