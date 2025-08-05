# RADM2 Chemical Mechanism: A Technical Guide

The Regional Acid Deposition Model 2 (RADM2) is a robust and computationally efficient chemical mechanism that is well-suited for a variety of applications.

## Mechanism Overview

RADM2 is a "lumped" chemical mechanism, which means that it groups volatile organic compounds (VOCs) into a smaller number of surrogate species. This simplification reduces the number of species and reactions that need to be solved, making the mechanism computationally less expensive than more explicit mechanisms like MOZART or RACM.

### Key Features

-   **15 stable inorganic species**
-   **4 organic "surrogate" species**
-   **23 photochemical reactions**

## Use Cases

-   **Regional Air Quality Studies**: RADM2 is a workhorse for regional air quality modeling, particularly for studies focusing on ozone and acid deposition.
-   **Educational Tool**: Its relative simplicity makes it an excellent tool for teaching the fundamentals of atmospheric chemistry and air quality modeling.
-   **Sensitivity Studies**: The low computational cost of RADM2 allows for a large number of sensitivity studies to be performed, for example, to assess the impact of different emissions scenarios.

## WRF-Chem `chem_opt`

-   **`chem_opt = 2`**: The standard RADM2 mechanism.
-   **`chem_opt = 101`**: The KPP version of RADM2. This version is more flexible and easier to modify.

## Coupling with Aerosols

RADM2 is often coupled with a simple aerosol scheme, such as the MADE/SORGAM aerosol model (`aer_opt = 1`). This combination provides a good balance between chemical detail and computational cost for studies where aerosol chemistry is not the primary focus.

## Limitations

-   **Simplified VOC Chemistry**: The lumped VOC scheme in RADM2 may not be appropriate for studies that require a detailed representation of specific VOCs or their oxidation products.
-   **Limited SOA Formation**: The standard RADM2 mechanism does not include a detailed representation of secondary organic aerosol (SOA) formation.

Despite these limitations, RADM2 remains a valuable tool for a wide range of atmospheric chemistry applications. Its computational efficiency and robustness make it an excellent choice for many research and operational forecasting scenarios.