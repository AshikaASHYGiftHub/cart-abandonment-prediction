## Dataset

### Raw Data
Due to file size constraints, the raw GA4 dataset is not included 
in this repository.

**Source:** Google BigQuery GA4 Obfuscated E-Commerce Public Dataset
- 300,000 event-level records
- January 2021 data from Google Merchandise Store
- Publicly available via Google BigQuery

**Access the dataset here:**
[Google BigQuery GA4 Public Dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=ga4_obfuscated_sample_ecommerce&t=events_20210131&page=table)

To query the data, you need a Google Cloud account with BigQuery access.

### Processed Data
`session_data_cleaned.csv` — Session-level aggregated dataset
- 29,178 session observations
- 15 engineered features
- Derived from 300,000 raw GA4 event-level records
