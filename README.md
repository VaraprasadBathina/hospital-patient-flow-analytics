# 🏥 Real-Time Healthcare Data Engineering Project
Hospital Patient Flow and Bed Occupancy Analytics (End to End Azure Pipeline)

This project is a full end to end real-time healthcare data engineering solution built on Azure, using modern architecture patterns like the Medallion Architecture, Delta Lake, Star Schema, SCD2, and real-time streaming with Kafka + Event Hub.

The goal is to simulate a real hospital network and build a scalable pipeline that can monitor:

Patient admissions

Department level load

Bed occupancy

Length of stay

Real time operational insights

This project goes far beyond traditional YouTube tutorials or Kaggle based projects, because it includes:

✔ Dirty data handling
✔ Schema evolution
✔ Real-time ingestion
✔ Slowly Changing Dimensions (SCD2)
✔ Star Schema modeling
✔ Automated orchestration with ADF
✔ Alerts and monitoring
✔ A fully queryable warehouse with Synapse and Power BI

## 📌 1. Project Overview

A fictional healthcare organization Midwest Health Alliance operates 7 hospitals and wants to track patient flow in real time. The project delivers:

A real time streaming data ingestion system

Bronze → Silver → Gold Medallion Architecture

Business-ready fact and dimension tables

Power BI dashboards for leadership teams

A fully automated Azure pipeline with alerts

## 🏗 2. Architecture
Data Simulation → Event Hub/Kafka → Databricks (Bronze/Silver/Gold)
           → Azure Data Factory → Synapse Analytics → Power BI

Layers

Bronze: Raw streaming data in Delta format

Silver: Cleaned, standardized data with schema handled

Gold: Star Schema tables with SCD2 logic

## 📊 3. Tools and Technologies Used
Component	Technology
Streaming	Azure Event Hub, Kafka
Storage	Azure Data Lake Gen2
Processing	Azure Databricks (PySpark + Delta)
Security	Azure Key Vault + RBAC
Orchestration	Azure Data Factory
Data Warehouse	Azure Synapse Analytics
Visualization	Power BI
Programming	Python (simulation)
## 🏥 4. Data Simulation (Live Patient Events)

A Python script generates one event per second:

patient_id (UUID)

gender, age

hospital_id (1–7)

department

bed_id

admission_time

discharge_time

Dirty Data Generation

To mimic real hospital systems, 5% of records include:

Invalid ages (101 to 150)

Future admission timestamps

Missing fields

This replicates real engineering challenges.

## 🔄 5. Streaming to Azure Event Hub

Event Hub namespace + Event Hub instance created

Code streams data continuously

Retention period set to 1 hour

Ensures real time ingestion pipeline

## 🪣 6. Bronze Layer (Raw Data)

Using Databricks Structured Streaming:

Read Kafka stream

Convert binary → raw JSON

Write Delta files to Bronze container

Use checkpoints for reliability

Bronze contains exactly what arrives, clean or dirty.

## 🧼 7. Silver Layer (Clean and Standardized Data)

Silver transformations include:

1. Schema Parsing

Define schema using StructType.

2. Data Cleaning

Fix invalid ages

Fix future timestamps

Handle missing fields

3. Schema Evolution

If new fields arrive, dynamically add null columns.

4. Write Delta Output

Silver stores business ready clean data.

## 🥇 8. Gold Layer (Star Schema + SCD2)

Gold transformations are batch (not streaming) because:

SCD2 requires historical state comparison

Fact tables need surrogate key lookup

Tables Created:
DIM Patient (SCD2)

Tracks demographic history:

Surrogate key

Gender, age

Hash key to detect changes

Effective from / to

Is_current flag

DIM Department

Stores department + hospital info.

FACT Patient Flow

Combines Silver data with surrogate keys:

patient_sk

department_sk

admission_time

discharge_time

length of stay hours

is_currently_admitted

## 🪄 9. Orchestration (Azure Data Factory)

ADF automates the Gold transformation:

Check Silver record count

If > 5 records, run Gold pipeline

Trigger runs every 5 minutes

Alerting

An Azure Monitor alert notifies on any pipeline failure.

## 🏛 10. Data Warehouse (Azure Synapse)
Steps:

Create Synapse workspace

Create dedicated SQL pool

Configure external data source to ADLS

Create external tables for Gold layer

Power BI reads these tables directly.

## 📈 11. Power BI Dashboard

Connected using DirectQuery for real time updates.

Dashboard shows:

Current bed occupancy

Average length of stay

Patient admissions by hour/day

Department load

Gender and age based KPIs

## 🤝 12. What Makes This Project Different

✔ Real time streaming, not batch
✔ Dirty data handling
✔ Schema evolution
✔ SCD2 implementation
✔ True enterprise architecture
✔ Orchestration + alerting
✔ End to end lifecycle from ingestion to reporting

This is designed for real world readiness and interview prep.

## 🚀 13. How to Run This Project (Setup Guide)
Prerequisites

Azure Subscription

Databricks Workspace

Event Hub

Synapse Workspace

Python 3.8+

Steps

Clone repository

Update environment variables

Run simulation script

Start Bronze, Silver, Gold notebooks in Databricks

Deploy ADF pipeline

Configure Synapse external tables

Connect Power BI to SQL endpoint


## ⭐ 14. Future Improvements

Add Delta Live Tables

Add ML model for occupancy prediction

Build hospital wide forecasting engine

## 🙌 15. Author

Varaprasad Bathina
Data Engineer | Azure | PySpark | SQL
