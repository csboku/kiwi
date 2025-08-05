# WRF-Chem and KPP

The Kinetic Pre-Processor (KPP) is a tool used to generate the code for chemical mechanisms in atmospheric models like WRF-Chem. It takes a set of chemical reactions and species as input and produces Fortran code that can be compiled into the main model.

## What is KPP?

-   **A Pre-Processor**: KPP is not a chemical mechanism itself. It's a tool that builds the mechanism code from a human-readable input file.
-   **Flexibility**: KPP allows researchers to easily add new species and reactions to a chemical mechanism without having to manually write complex Fortran code.
-   **Standardization**: It provides a standardized way to define and implement chemical mechanisms, which makes it easier to compare different mechanisms.

## The KPP Workflow in WRF-Chem

1.  **Define the Mechanism**: The chemical mechanism is defined in a `.kpp` file (e.g., `racm.kpp`). This file lists all the chemical species and the reactions they are involved in.
2.  **Run KPP**: The KPP tool is run on the `.kpp` file. It generates a set of Fortran files (`_kpp_*.f`) that contain the subroutines for the chemical kinetics, such as the calculation of reaction rates and the integration of the chemical equations.
3.  **Compile into WRF-Chem**: These generated Fortran files are then compiled along with the rest of the WRF-Chem source code. The `chem_opt` parameter in the `namelist.input` file tells WRF-Chem which KPP-generated mechanism to use.

Many of the chemical mechanisms in WRF-Chem are implemented using KPP, including [RACM](./wrfchem_racm.md), some versions of [CBMZ](./wrfchem_cbmz.md), and [MOZART](./wrfchem_mozart_kpp.md).

## The `.kpp` File Structure

A `.kpp` file has a specific structure:

-   **`#include` statements**: These are used to include other files, such as files containing rate constants.
-   **`#def` statements**: These are used to define macros.
-   **`#FAMILIES`**: This section is used to define chemical families, which can be used to simplify the definition of reactions.
-   **`#EQUATIONS`**: This is the main section of the file, where the chemical reactions are defined. Each reaction is written as an equation, with the reactants on the left and the products on the right, separated by a colon and the rate constant.

Example of a reaction in a `.kpp` file:

```
O3 + NO = NO2 : k_o3_no ;
```

This line defines the reaction between ozone (O3) and nitric oxide (NO) to produce nitrogen dioxide (NO2), with the rate constant `k_o3_no`.

## Creating a New Mechanism with KPP

To create a new chemical mechanism for WRF-Chem, you would need to:

1.  **Create a new `.kpp` file**: Define your species and reactions in the KPP syntax.
2.  **Integrate with the WRF build system**: This involves modifying the WRF-Chem `Registry` file to add the new species, and updating the build scripts to run KPP for your new mechanism.
3.  **Add a new `chem_opt`**: You would need to assign a new number for your mechanism in the `chem_opt` list.

This is an advanced topic that requires a deep understanding of both atmospheric chemistry and the WRF-Chem source code. For most users, it is sufficient to use the chemical mechanisms that are already included with the model.
