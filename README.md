
# PandaPodium Insights Hub

> **About:** **PandaPodium Insights Hub: End-to-end cycling e-commerce analytics platform, inspired by [PandaPodium.cc](https://pandapodium.cc). Showcases batch & stream processing (PySpark, PyFlink), IaC (Terraform), orchestration (Airflow), dbt, data lake/warehouse (S3, Glue, Athena), & viz (Tableau/Metabase) for product, sales, & customer insights.**


[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
![Project Status](https://img.shields.io/badge/status-work--in--progress-orange)

## Project Status
🚧 **Work in Progress**: This project is currently under active development. Features and documentation may change frequently until a stable release is finalized.




## Project Overview

The PandaPodium Insights Hub is a comprehensive, end-to-end data analytics platform designed to simulate and analyze the operations of a cycling e-commerce business, with specific inspiration drawn from [PandaPodium.cc](https://pandapodium.cc/). This project demonstrates the creation of a scalable data infrastructure capable of ingesting, processing, transforming, and visualizing data from various sources to derive actionable business insights.

It covers the entire data lifecycle, from raw data ingestion (batch and stream) to sophisticated data modeling and interactive dashboarding, leveraging a modern, cloud-native (AWS) tech stack.

-----

## Motivation

Panda Podium aims to provide "value bike parts" and the "best service of any bike part website on the internet" to a global customer base, sourcing products primarily from Far Eastern manufacturers. As a growing e-commerce entity, the ability to effectively harness data is crucial for:

  * Understanding customer behavior and preferences.
  * Optimizing product catalog and inventory.
  * Analyzing sales trends and regional performance.
  * Enhancing marketing effectiveness.
  * Improving overall operational efficiency and service delivery.

This project was built as a speculative demonstration of how a modern data engineering platform can directly support these goals, providing Panda Podium with the tools to make data-driven decisions and further enhance their competitive edge in the cycling market. It showcases a practical application of a robust data stack to solve real-world e-commerce challenges.

-----

## Tech Stack

This project utilizes a range of modern data engineering tools and technologies:

  * **Cloud Provider:** AWS
  * **Infrastructure as Code (IaC):** Terraform
  * **Workflow Orchestration:** Apache Airflow
  * **Containerization:** Docker
  * **Data Lake:**
      * Production: Amazon S3
      * Development: Local file system / PostgreSQL
  * **Data Warehouse:**
      * Production: Amazon S3 (with AWS Glue Data Catalog & Amazon Athena)
      * Development: PostgreSQL
  * **Data Ingestion (Batch):**
      * Python
      * `dlt` (data load tool)
  * **Batch Processing:**
      * PySpark (with local and Amazon EMR configurations)
  * **Data Transformation:**
      * `dbt` (data build tool)
  * **Testing & Documentation (Data Models):** `dbt`
  * **Streaming Data Ingestion & Processing:**
      * Messaging (Dev): Kafka (Redpanda)
      * Messaging (Prod): Amazon MSK (Managed Streaming for Apache Kafka)
      * Stream Processing (Dev): PyFlink (local cluster)
      * Stream Processing (Prod): AWS Managed Service for Apache Flink
      * Python Kafka Clients
  * **Analytics:** Python (Pandas, NumPy), SQL
  * **Data Visualization:**
      * Development: Metabase
      * Production: Tableau Public
  * **DevOps & Workflow:**
      * Git (for version control)
      * Terraform Workspaces (for environment isolation: dev/prod)
      * Integrate CI/CD pipelines for automated testing and deployment
      * Use `Make`

-----

## Architecture

*(This section is a placeholder. I'll create an architecture diagram and embed it here. Perhaps using tools like diagrams.net (draw.io), Lucidchart, or even PowerPoint/Google Slides and export as an image.)*

-----

| Data Sources | ----\> | Ingestion | ----\> | Storage | ----\> | Processing & | ----\> | Visualization |
| (Scraped, | | (dlt, Kafka/MSK, | | (S3 Data Lake, | | Transformation | | (Metabase, |
| Synthetic Ecom, | | PyFlink for Stream)| | Postgres for Dev) | | (PySpark, dbt, | | Tableau Public) |
| Public Streams) | --------------------       -------------------- | Athena, Glue) | --------------------

-----

```bash
    ^ |


| |
\------------------------------------ Orchestration (Airflow) -------------------------
|
|
\-------------------------------
| Infrastructure (Terraform) |
| Containerization (Docker) |
\-------------------------------

```

**Brief Explanation:**

*   **Data Sources:** Product catalog data is scraped from [PandaPodium.cc](https://pandapodium.cc/). Sales transactions, customer data, and web clickstream data are synthetically generated or adapted from public e-commerce datasets. Public real-time streams (e.g., Wikimedia EventStreams, GTFS-Realtime) are used for the streaming component.
*   **Ingestion:** Batch data is loaded using `dlt` and Python scripts orchestrated by Airflow. Streaming data is ingested via Kafka/MSK and processed by PyFlink.
*   **Storage:** Raw and processed data resides in Amazon S3, forming the data lake. PostgreSQL is used for development warehousing to minimize cloud costs.
*   **Processing & Transformation:** PySpark handles large-scale batch transformations. `dbt` is used for SQL-based data modeling and transformation within the data warehouse (Athena for prod, Postgres for dev). AWS Glue Data Catalog provides schema for Athena.
*   **Orchestration:** Apache Airflow manages and schedules batch data pipelines.
*   **Infrastructure:** All AWS infrastructure is provisioned and managed using Terraform. Docker is used for containerizing applications/services where appropriate.
*   **Visualization:** Metabase is used for development dashboards, while Tableau Public is used for production-ready, shareable visualizations.

---

## Data Sources

_(Still in data exploration and highly likely additional/removal of data sources will happen.)_

1.  **Product Catalog (Scraped):**
    *   Source: [PandaPodium.cc](https://pandapodium.cc/) (publicly available product information)
    *   Fields: Product Name, Prices, Brand, Image URLs, Descriptions, Specifications, Categories, Stock Status, etc.
    *   Method: Web scraping (ensure ethical considerations and respect for `robots.txt` if applicable for a real-world scenario).
2.  **Sales Transactions (Synthetic/Public):**
    *   Source: Adapted from public e-commerce datasets (e.g., Kaggle datasets) and enhanced with synthetic data generation (Faker).
    *   Fields: Transaction ID, Customer ID, Product ID (linked to scraped catalog), Quantity, Price, Timestamp, Region.
3.  **Customer Data (Synthetic/Public):**
    *   Source: Adapted from public e-commerce datasets and enhanced with synthetic data generation (Faker).
    *   Fields: Customer ID, Name, Region, Signup Date, Age (simulated).
4.  **Web Clickstream Data (Synthetic):**
    *   Source: Generated synthetically using Python (Faker, random) to simulate user interactions.
    *   Fields: Session ID, User ID, Timestamp, Event Type (page_view, product_view, add_to_cart), Product ID (linked to scraped catalog).
    *   *This data is primarily intended for the streaming pipeline.*
5.  **Public Real-Time Streams (for Streaming Component):**
    *   Source: Examples include Wikimedia EventStreams (recent changes) or GTFS-Realtime feeds from public transit agencies.
    *   Purpose: To demonstrate real-time data ingestion and processing capabilities.

---

## Key Features

*   **Automated Data Pipelines:** End-to-end batch and streaming data pipelines orchestrated by Apache Airflow.
*   **Scalable Data Processing:** Utilizes PySpark for distributed batch processing and PyFlink for scalable stream processing.
*   **Infrastructure as Code:** All cloud infrastructure managed by Terraform for reproducibility and version control.
*   **Modular Data Modeling:** `dbt` for robust, version-controlled, and testable data transformations in the warehouse.
*   **Environment Parity:** Clear separation between `dev` and `prod` environments using Terraform workspaces and distinct configurations.
*   **Interactive Dashboards:**
    *   Development dashboards in Metabase for quick analytics and monitoring.
    *   Production-quality dashboards in Tableau Public for stakeholder presentation.
*   **Comprehensive Analytics:** Enables analysis of sales performance, customer segmentation, product trends, and (simulated) real-time user activity.

---

## Directory Structure

```

pandapodium-insights-hub/
├── airflow/                \# Airflow DAGs and related configurations
│   ├── dags/
│   └──...
├── dbt/            \# dbt project for data transformations
│   ├── models/
│   ├── tests/
│   ├── snapshots/
│   └── dbt_project.yml
├── docker/                 \# Dockerfiles and docker-compose files
│   ├── airflow/
│   ├── redpanda/
│   └──...
├── notebooks/              \# Jupyter notebooks for exploration and ad-hoc analysis
├── scripts/                \# Helper scripts (e.g., data generation, scraping)
│   ├── scraping/
│   └── data_generation/
├── src/                    \# Source code for data processing jobs
│   ├── batch/              \# PySpark jobs
│   └── streaming/          \# PyFlink jobs
├── terraform/              \# Terraform configurations for infrastructure
│   ├── modules/
│   ├── dev/
│   └── prod/
├── tests/                  \# Unit and integration tests for src code
├──.gitignore
├── LICENSE
└── README.md

````

---

## Setup and Installation

*(This placeholder section will be highly specific to actual project's setup. Clear, step-by-step instructions will be added later on.)*

**Prerequisites:**

*   AWS Account & AWS CLI configured
*   Terraform installed
*   Docker & Docker Compose installed
*   Python (specify version, e.g., 3.8+) & pip
*   `dbt-core` and relevant `dbt` adapter (e.g., `dbt-postgres`, `dbt-athena`)
*   Apache Airflow (can be run via Docker)
*   (Optional) Redpanda/Kafka installed or running via Docker for local streaming dev
*   (Optional) Metabase installed or running via Docker
*   (Optional) Tableau Public Desktop

**Steps:**

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/pizofreude/pandapodium-insights-hub.git](https://github.com/pizofreude/pandapodium-insights-hub.git)
    cd pandapodium-insights-hub
    ```

2.  **Set up Python Virtual Environment (Recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    pip install -r requirements.txt # Create a requirements.txt for Python dependencies
    ```

3.  **Configure AWS Credentials:**
    Ensure your AWS CLI is configured with the necessary permissions. Terraform will use these credentials.
    ```bash
    aws configure
    ```

4.  **Deploy Infrastructure with Terraform:**
    *   Navigate to the appropriate environment directory (e.g., `terraform/dev/`).
    *   Initialize Terraform:
        ```bash
        terraform init
        ```
    *   (Optional) Plan the deployment:
        ```bash
        terraform plan
        ```
    *   Apply the configuration:
        ```bash
        terraform apply
        ```
    *   *Note: You might need to manually create an S3 bucket for Terraform remote state backend before running `terraform init` if you configure it that way.*

5.  **Set up `dbt` Project:**
    *   Navigate to the `dbt/` directory.
    *   Configure your `profiles.yml` (typically located in `~/.dbt/profiles.yml`) with connection details for your dev (Postgres) and prod (Athena) environments.
    *   Install dbt dependencies:
        ```bash
        dbt deps
        ```

6.  **Run Data Generation/Scraping Scripts:**
    *   Execute scripts in the `scripts/` directory to populate initial data (e.g., scrape product catalog, generate synthetic sales data).
    *   *(Provide specific commands here)* [TO-DO]

7.  **Start Local Services (Docker Compose):**
    If using Docker for Airflow, Redpanda, Postgres (dev), Metabase:
    ```bash
    docker-compose up -d # From the directory containing your docker-compose.yml
    ```

8.  **Configure Airflow Connections:**
    Once Airflow is running, configure connections to AWS, Spark, etc., through the Airflow UI.

---

## Running the Project

1.  **Trigger Airflow DAGs:**
    *   Access the Airflow UI (typically `http://localhost:8080`).
    *   Unpause and trigger the relevant DAGs for batch ingestion, PySpark processing, and `dbt` model runs.

2.  **Run `dbt` Models (Manual):**
    *   Navigate to `dbt/`.
    *   To run models for development:
        ```bash
        dbt run --target dev
        dbt test --target dev
        ```
    *   To generate documentation:
        ```bash
        dbt docs generate --target dev
        dbt docs serve --target dev
        ```

3.  **Start Streaming Pipeline:**
    *   Ensure Redpanda/MSK is running and topics are created.
    *   Submit your PyFlink job.
    *   *(Provide specific commands for submitting Flink jobs, e.g., using `flink run`)* [TO-DO]

4.  **Access Dashboards:**
    *   **Metabase (Dev):** Connect Metabase to your development PostgreSQL database and build/view dashboards.
    *   **Tableau Public (Prod):** [Connect Tableau Public to Athena](https://youtu.be/b5zKzpif-wM?si=-WXqMV7-ltoHnbnu) (or use data extracts) to create and view production dashboards.

---

## Example Dashboards

*(Include screenshots & links to Tableau Public dashboards here. Describe what insights they provide.)* [TO-DO]

**Example 1: Sales Overview Dashboard**
*   Shows total sales, sales by product category, sales by region over time.
*   Helps identify top-performing products and regions.

**Example 2: Customer Segmentation Dashboard**
*   Segments customers based on purchase history, frequency, and value.
*   Aids in targeted marketing campaigns.

---

## Future Enhancements

*   Implement more sophisticated Machine Learning models (e.g., sales forecasting, recommendation engine).
*   Expand real-time analytics capabilities (e.g., real-time inventory tracking, fraud detection).
*   Add more comprehensive data quality checks and alerting.
*   Develop a more interactive front-end for data exploration.

---

## Contributing

While this is primarily a portfolio project, contributions, suggestions, and feedback are welcome :) Please feel free to open an issue or submit a pull request.
If possible, kindly follow this [conventions](https://dev.to/varbsan/a-simplified-convention-for-naming-branches-and-commits-in-git-il4).

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'feat: Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## License

This project is licensed under the Apache 2.0 License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

*   [PandaPodium.cc](https://pandapodium.cc/) for the inspiration and publicly available product information.
*   The open-source community for the amazing tools and libraries used in this project.
*   Publicly available datasets from Kaggle, NYC Citi Bike, Wikimedia, GTFS providers, etc.

---
