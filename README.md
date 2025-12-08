# 🎓 Identifying Key Policy Levers to Improve Immigrant Educational Attainment

## 🌟 About This Project

The problem addressed is the persistent educational attainment gap observed between foreign-born (immigrant) and native-born U.S. residents.

This analysis provides quantifiable, evidence-based insights to inform policy by using a stable Weighted Least Squares (WLS) model to isolate the effects of socioeconomic, structural, and linguistic factors.

## ⚙️Data Source and Methodology

### Data Source

* **Source**: ACS PUMS 1-Year Estimates, 2023.

* **Geography**: State of Florida.
  
* **Key Variables Used**: *SCHL* (Education), *NATIVITY* (Birth Status), *POVPIP* (Income Proxy), and *PWGTP* (Weight).

Final Model Specification (WLS)The model was significantly enhanced from the initial concept to include two critical language barrier variables and a technique to ensure stability:

$\text{Years of Schooling} = \beta_0 + \beta_1 (\text{Is\_Immigrant}) + \dots + \beta_5 (\text{Limited\_English\_Household}) + \epsilon$$

## 🧹 Data Preparation and Feature Engineering

This project transforms the raw PUMS codes into the precise variables required for the regression model.

* **Sample Definition**: The dataset was filtered to include only individuals aged 25 and older (AGEP >= 25) to accurately capture completed education.

* **Dependent Variable** **(Y)** (Years of Schooling): The categorical SCHL codes (e.g., Bachelor's Degree) were mapped to a continuous numerical variable representing the total years of schooling (e.g., 16 years).

* **Independent Variable $\mathbf{X}_1$ (Is_Immigrant)**:

  1. This variable is a binary dummy created from the NATIVITY variable.

  2. Classification: $1 = \text{Immigrant (Foreign-Born)}$ and $0 = \text{U.S. Born}$ (Reference Group).
  
*Note: Due to the age filter (25+), the **NOP** variable for second-generation status was unusable, necessitating this robust binary proxy.*

* **Weighting**: The *PWGTP* (Person Weight) variable is applied in the MLR to ensure the sample estimates accurately reflect the true population.

A. **Key Variables Added**

The model's explanatory power was improved by adding two key linguistic variables:

* $\text{Limited\_English\_Household}$: Binary variable identifying low English proficiency in the household.

* $\text{Speaks\_Other\_Language}$: Binary control for language spoken at home.

B. **Statistical Stabilization**

The model required stabilization to ensure reliable coefficients:

Problem: The initial model suffered from severe multicollinearity.

Solution: The socioeconomic variable $\text{POVPIP}$ was mean-centered to create $\text{POVPIP}_c$. This stabilized the model, reducing the Condition Number from $2.03e+03$ (Problematic) to $\mathbf{578.}$ (Stable).

## 📊 Results

* The **Limited English Household Barrier** is more than double the magnitude of the residual Immigrant Gap, proving its importance as a policy focus.

* The potential gain is (3.2 years) which more than four times the size of the Dominant Language Barrier (0.74 years).

## 📝Policy

* Prioritize and significantly fund adult english as a Second Language (ESL) and family literacy programs.

* Provide dedicated, language-appropriate financial aid and college Counseling for immigrant families.

* Develop specialized high School integration pathways immigrant students.
