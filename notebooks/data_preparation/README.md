# 🧹 Data Cleaning and Feature Engineering Documentation

This document details the process to transform the raw ACS PUMS 2023 data into the final, analytical dataset (`ACSPUMS_CLEANED`) used for the Multiple Linear Regression (MLR) model.

1. **Data Cleaning and Sample Definition**

    The objective of this phase was to define the precise population ($\text{aged 25+}$) and ensure the integrity of the core variables.

    A. **Initial Filtering and Scope**

   * **Age Restriction**: The dataset was filtered to include only individuals aged 25 years and older ($\text{AGEP} \ge 25$). This is necessary to ensure the analysis measures completed educational attainment.

   * **Missing Data Removal**: Records containing PUMS missing/non-applicable codes (e.g., $\text{-1}$ or $\text{0}$) were removed from the key analytical columns:
     * SCHL (Educational Attainment)
     * NATIVITY (Birth Status)
     * POVPIP (Income Proxy)

   * **Junk Column Removal**: Redundant or unused columns resulting from the initial CSV export, such as $\text{PWGTP.1}$ and $\text{Unnamed: 14}$, were dropped.

    B. **Analytical Variables Summary**

    After cleaning, the analysis proceeded with the following key variables:

    | Variable Name | PUMS Source | Role |
    | :--- | :--- | :--- |
    | **Years\_of\_Schooling** | SCHL | Dependent Variable (Y) |
    | **Is\_Immigrant** | NATIVITY | Independent Variable (X1) |
    | **POVPIP** | POVPIP | Control Variable (X2) |
    | **PWGTP** | PWGTP | Survey Weight |

2. **Feature Engineering: Variable Creation**

    This phase involved creating the numerical variables required for the Multiple Linear Regression model.

    A. **Dependent Variable ($\text{Y}$): Years of Schooling**

    The categorical codes in the $\text{SCHL}$ column were mapped to a continuous numerical scale representing estimated total years of schooling. This transformation is required for Linear Regression.

   * **Transformation Logic:** $\text{numpy.select}$ was used to map ranges of $\text{SCHL}$ codes to discrete year values based on common PUMS methodology:

     * $\text{SCHL} = \text{1-11} \implies \text{Years} = \text{1-11}$

     * $\text{SCHL} = \text{12-16} \implies \text{Years} = \text{12}$ (High School Equivalent)

     * $\text{SCHL} = \text{20} \implies \text{Years} = \text{16}$ (Bachelor's Degree)

     * $\text{SCHL} = \text{23-24} \implies \text{Years} = \text{20}$ (Doctorate Degree)

   * **Result**: The new variable `Years_of_Schooling` was created.

    B. **Independent Variable ($\text{X}_1$): Is_Immigrant (Dummy Variable)**

    The analytical variable was created to compare the newest immigrant generation against the native-born population.

   * **Source**: The `NATIVITY` variable was used to distinguish place of birth.

   * **Classification**: A binary dummy variable named `Is_Immigrant` was created:

     * $1$:**Immigrant (Foreign-Born)** ($\text{NATIVITY} = \text{2}$)

     * $0$: **U.S. Born (Reference Group)** ($\text{NATIVITY} = \text{1}$)

   * **Justification**: The desired three-category classification (Native, Second-Gen, First-Gen) was not possible because the $\text{NOP}$ (Nativity of Parent) variable is coded as N/A for persons greater than 17 years old and was therefore unusable for the $\text{25+}$ sample.

    C. Control Variable ($\text{X}_2$): $\text{POVPIP}$ (Income-to-Poverty Ratio)
  
    * Original Action: The $\text{POVPIP}$ variable was initially included as is.
  
    * Correction (Feature Engineering - Fix for Multicollinearity):
      * The interaction term ($\text{Is\_Immigrant}:\text{POVPIP}$) introduced severe multicollinearity ($\text{Condition\ No.}=2,030$).
  
      * To resolve this, $\text{POVPIP}$ was mean-centered (i.e., $\text{POVPIP}_c = \text{POVPIP} - \text{POVPIP}_{\text{mean}}$).
  
      * The final model uses this centered term ($\text{POVPIP}_c$) and the centered interaction term ($\text{Immigrant\_POVPIP\_c}$).

    D. Added Barrier Variables ($\text{X}_3$, $\text{X}_4$): Language Barriers

    * Rationable: To improve explanatory power and isolate the specific barriers immigrants face, two key linguistic variables were added to the model.ed to the model.

    * $\text{Limited\_English\_Household}$ (Source: $\text{LNGI}$): Binary dummy variable (1 = $\text{No\ one\ 14\ and\ over\ speaks\ English\ or\ speaks\ English\ 'very\ well'}$).

    * $\text{Speaks\_Other\_Language}$ (Source: $\text{LANX}$): Binary dummy variable (1 = $\text{Yes,\ speaks\ another\ language\ spoken\ at\ home}$).
