# Multi-Tool AI Agent for Bangladesh

**Author:** Shah Aymaan Ibtihal  
**Ostad Module 22 Assignment**

## Overview

This project uses LangChain AgentExecutor to answer questions using
three Bangladesh datasets and a web-search tool.

Gemini interprets questions, generates SQL, and summarizes results.
SQLite executes database queries. Tavily retrieves web information.

## Tools

- **InstitutionsDBTool:** Queries educational institution records,
  including institution types, locations, management, and education levels.
- **HospitalsDBTool:** Queries hospital and health-facility records,
  including names, types, agencies, and locations.
- **RestaurantsDBTool:** Queries restaurant listings, addresses,
  coordinates, ratings, and review counts.
- **WebSearchTool:** Searches for general knowledge, definitions,
  policies, and cultural context.

The main agent selects the relevant tool automatically and can use
multiple tools for a question.

## Datasets

- [Institutional Information of Bangladesh](https://huggingface.co/datasets/Mahadih534/Institutional-Information-of-Bangladesh)
- [All Bangladeshi Hospitals](https://huggingface.co/datasets/Mahadih534/all-bangladeshi-hospitals)
- [Bangladeshi Restaurant Data](https://huggingface.co/datasets/Mahadih534/Bangladeshi-Restaurant-Data)

## Database Preparation

The notebook loads the datasets, standardizes column names, preserves
missing values, assigns SQLite column types, and creates these databases:

| Database | Table | Records in the tested version |
|---|---|---:|
| institutions.db | institutions | 34,901 |
| hospitals.db | hospitals | 38,886 |
| restaurants.db | restaurants | 10,758 |

The restaurant dataset contained 1,945 fully duplicated rows, which
were removed.

Database files are generated inside the `databases` folder when
the notebook runs. Database tools use read-only connections.

## How to Run

1. Download the `.ipynb` notebook from this repository.
2. Open Google Colab and upload the notebook.
3. Add the following entries under Colab Secrets:
   - `CLOUD_API_KEY`: your Gemini API key.
   - `TAVILY_API_KEY`: your Tavily API key.
4. Enable Notebook access for both secrets.
5. Run the notebook cells from top to bottom. Installation cells
   install the required packages.
6. After setup, enter a question in a new code cell, for example:

   `ask_agent("How many colleges are in Dhaka district?")`

Internet access and valid Gemini and Tavily API access with available
quota are required. The tested Gemini model is `gemini-3.5-flash-lite`.

## Example Questions

- How many colleges are in Dhaka district?
- How many Community Clinics are in Dhaka district?
- Show the five most-reviewed restaurant listings with a rating of at least 4.
- What is the role of DGHS in Bangladesh? Use official sources only.
- Using the provided datasets, report the numbers of colleges and
  Community Clinics in Dhaka district.

## Testing

The notebook includes tool-routing tests, a mixed-dataset question,
an unavailable-data question, a zero-match query, and a greeting.

Manual SQL verified 227 college records and 159 Community Clinic
records in Dhaka district.

## Limitations

- Answers describe the supplied datasets, not complete or current
  national inventories.
- The hospital dataset includes clinics, offices, diagnostic centers,
  and other facilities. Unspecified hospital queries use categories
  containing “Hospital”, including combined hospital/clinic categories.
- Bed and doctor counts are unavailable.
- Restaurant cuisine, menus, and opening hours are unavailable.
- Restaurant location searches use address text rather than verified
  district boundaries.
- Generated SQL and web-based answers may still require verification.
