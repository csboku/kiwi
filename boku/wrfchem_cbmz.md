# CBMZ Chemical Mechanism

The Carbon Bond Mechanism, Version Z (CBMZ) is a gas-phase chemical mechanism that is widely used in air quality models, including WRF-Chem. It is a "lumped-structure" mechanism, which means it simplifies the chemistry by grouping organic compounds based on their internal bond types.

## Key Features

-   **Computational Efficiency**: CBMZ is known for its computational efficiency, making it a popular choice for air quality forecasting and research.
-   **Lumped Structure**: The lumping of organic species makes the mechanism smaller and faster to run than more explicit mechanisms.
-   **Coupling with Aerosols**: In WRF-Chem, CBMZ is typically coupled with an aerosol module, such as MOSAIC or MADE/SORGAM, to provide a more complete picture of atmospheric composition. For more details on the CBMZ/MOSAIC coupling, see the [CBMZ/MOSAIC In-Depth](./wrfchem_cbmz_mosaic.md) guide.

## Use Cases

-   **Air Quality Forecasting**: Due to its computational efficiency, CBMZ is a popular choice for operational air quality forecasting.
-   **Regulatory Modeling**: It is often used for regulatory modeling applications, such as assessing the impact of emission control strategies.
-   **Studies with Aerosols**: When coupled with an aerosol module like MOSAIC, CBMZ is a powerful tool for studying the interactions between gas-phase chemistry and aerosols.

## WRF-Chem `chem_opt`

There are several `chem_opt` values for CBMZ, depending on the aerosol module you want to use:

-   `chem_opt = 7`: CBMZ with MOSAIC aerosols (4 bins).
-   `chem_opt = 8`: CBMZ with MOSAIC aerosols (8 bins).
-   `chem_opt = 35`: CBMZ with MADE/SORGAM aerosols.
-   `chem_opt = 503`: CBMZ with MAM3 aerosols.

## KPP Implementation

While some versions of CBMZ in WRF-Chem are implemented using the standard WRF-Chem framework, there are also versions that use the Kinetic Pre-Processor (KPP). The KPP implementation provides greater flexibility for modifying the mechanism.

For more detailed information, refer to the official WRF-Chem User's Guide and the scientific literature on the CBMZ mechanism.
