# WRF-Chem and KPP: A Technical Guide

The Kinetic Pre-Processor (KPP) is a powerful tool that forms the backbone of many of the chemical mechanisms in WRF-Chem. This guide provides a more technical look at KPP and its integration with WRF-Chem.

## The KPP Engine

KPP takes a set of text files describing a chemical mechanism and generates a Fortran solver for that mechanism. The key components of the KPP engine are:

-   **The Parser**: Reads the `.kpp`, `.spc`, and `.eqn` files and builds an internal representation of the chemical mechanism.
-   **The Integrator**: KPP includes a library of numerical integrators for solving the stiff ordinary differential equations (ODEs) that describe chemical kinetics. The choice of integrator is specified in the `.kpp` file (e.g., `#INTEGRATOR rosenbrock`).
-   **The Code Generator**: Generates the Fortran code for the chemical solver, including the Jacobian matrix and other components required by the integrator.

## The WRF-Chem/KPP Interface

The link between KPP and WRF-Chem is the WRF-Chem/KPP interface, which is a set of scripts and Fortran routines that:

-   **Prepares the KPP input**: The interface scripts collect the necessary information from the WRF-Chem registry and the KPP mechanism files.
-   **Runs KPP**: The scripts then run the KPP executable to generate the chemical solver.
-   **Provides the "glue" code**: The interface includes Fortran routines that pass information (e.g., temperature, pressure, species concentrations) between the main WRF-Chem model and the KPP-generated solver.

## Advanced KPP Usage

### Modifying a Mechanism

To modify an existing KPP-based mechanism, you will need to edit the corresponding files in the `chem/KPP/mechanisms` directory.

-   **Adding a new species**: Add the species to the `.spc` file and to the `Registry/Registry.chem` file.
-   **Adding a new reaction**: Add the reaction to the `.eqn` file. You may also need to define a new rate constant.

### Creating a New Mechanism

Creating a new KPP-based mechanism from scratch is a major undertaking. The basic steps are:

1.  Create a new directory in `chem/KPP/mechanisms`.
2.  Create the `.kpp`, `.spc`, and `.eqn` files for your new mechanism.
3.  Add the new species to the `Registry/Registry.chem` file.
4.  Add a new `chem_opt` for your mechanism in the WRF-Chem build system.

### KPP Output

When you compile WRF-Chem with a KPP-based mechanism, the KPP tool will generate a set of Fortran files in the `chem/KPP/` directory. These files include:

-   `_kpp_`*mechanism*`_Jac.f`: The Jacobian matrix.
-   `_kpp_`*mechanism*`_Integrator.f`: The numerical integrator.
-   `_kpp_`*mechanism*`_Rates.f`: The chemical reaction rates.

By understanding the KPP workflow and the structure of the KPP files, advanced users can gain a deeper understanding of the chemical mechanisms in WRF-Chem and even develop their own customized mechanisms.