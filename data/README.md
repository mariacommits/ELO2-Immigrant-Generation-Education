# 🌎 Data Source Descriptive Document

This project utilizes publicly available survey data collected by the **U.S. Census Bureau** through the **American Community Survey (ACS)**.

1. **The American Community Survey (ACS)**

    * **Source**: The American Community Survey (ACS) is an ongoing, nationwide survey that provides data every year, giving communities the current information they need to plan investments and services.

    * **Data Type**: We are using the **Public Use Microdata Sample (PUMS)**, which consists of records for individuals and households selected from the full ACS survey. PUMS data allows researchers to create customized tables and conduct detailed statistical analysis.

    * **Survey Weights**: Due to the sampling nature of the ACS, the variable *PWGTP* (Person Weight) is included with every record. This weight will be applied in the regression (using Weighted Least Squares) to ensure that the sample results accurately represent the entire population of Florida.

2. **Specific Data File Context**
    * **Dataset Used**: ACS PUMS 1-Year Estimates, **2023**.

    * **Geographic Scope**: The raw data file was downloaded and filtered specifically for the state of Florida.

    * **Key Collection Variables**: The analysis relies on three primary variables collected by the Census Bureau:

        * **Educational Attainment** (*SCHL*).

        * **Nativity Status** (*NATIVITY*).

        * **Income Proxy** (*POVPIP*).
