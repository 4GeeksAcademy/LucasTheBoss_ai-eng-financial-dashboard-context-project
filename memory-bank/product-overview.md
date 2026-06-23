# Product Overview

## What this repository is

This project is a financial metrics dashboard starter with:

- a FastAPI backend that serves deterministic mock financial movement data and analytical endpoints
- a React + TypeScript frontend that renders executive-style KPI cards and charts

The repository is currently oriented toward training, repository stewardship, and safe maintenance workflows rather than production-grade financial operations.

## What the product currently does

The backend can return:

- raw financial movement records
- filter facets for available categories and date bounds
- grouped summaries by day, week, or month
- top categories for a selected operation type
- current-versus-previous period net comparison
- alerts for unusually high outcomes relative to historical averages
- B2B-only and B2C-only filtered views

The frontend currently displays:

- a dashboard header
- total income, total outcome, profit, and profit margin KPI cards
- an income versus outcome trend chart
- a profit margin trend chart
- loading and fetch-error states

## Current operating model

- Data is generated in memory on demand; there is no database.
- The backend is the source of raw records and also exposes analytical endpoints not yet used by the frontend.
- The frontend currently pulls only `/api/metrics` and computes visible dashboard metrics locally.

## Product boundaries

- No authentication or user-specific views
- No persistence layer
- No create/update/delete workflows
- No historical import or external financial system integration
- No evidence of production deployment configuration in the repository

## Important contributor note

Do not assume the dashboard's current visible labels fully describe the actual data window. The backend generates a rolling year of seeded data relative to the current date, while the frontend header still shows a fixed annual label.