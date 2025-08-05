# MOZART and KPP

The Model for Ozone and Related chemical Tracers (MOZART) is one of the most comprehensive chemical mechanisms available in WRF-Chem. Its implementation relies on the Kinetic Pre-Processor (KPP) to translate the complex chemistry into efficient Fortran code.

## The KPP Workflow for MOZART

When you compile WRF-Chem with a MOZART `chem_opt`, the build system uses KPP to generate the chemical solver. This process involves several key files:

-   **`mozart.spc`**: This file defines all the chemical species used in the mechanism.
-   **`mozart.eqn`**: This is the core of the mechanism, containing the list of all chemical reactions and their rate constants.
-   **`mozart.kpp`**: This file controls the KPP code generation process, specifying the numerical integrator and other options.
-   **`mozart_wrfkpp.equiv`**: This file maps the species names used in the KPP files to the corresponding variable names in the WRF-Chem registry.

The KPP tool reads these files and generates a set of Fortran routines that are then compiled and linked into the main WRF-Chem executable.

## Key Advantages of the KPP Approach

-   **Flexibility**: Researchers can easily modify the MOZART mechanism by editing the `.spc` and `.eqn` files, without having to change the core WRF-Chem code.
-   **Accuracy**: KPP uses proven numerical methods to solve the stiff ordinary differential equations that describe chemical kinetics.
-   **Efficiency**: The generated code is optimized for performance.

## Namelist Settings

To use the KPP-based MOZART mechanism, you will typically set the following in your `namelist.input`:

-   `chem_opt = 111`: MOZART gas-phase chemistry.
-   `chem_opt = 112`: MOZART gas-phase chemistry coupled with the GOCART aerosol model (MOZCART).

This KPP-based implementation makes MOZART a powerful and flexible tool for studying a wide range of atmospheric chemistry problems.
