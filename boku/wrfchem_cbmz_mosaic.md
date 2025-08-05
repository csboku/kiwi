# CBMZ/MOSAIC In-Depth

The combination of the Carbon Bond Mechanism, Version Z (CBMZ) for gas-phase chemistry and the Model for Simulating Aerosol Interactions and Chemistry (MOSAIC) for aerosols is a powerful and widely used configuration in WRF-Chem.

## MOSAIC: A Sectional Aerosol Model

MOSAIC is a sectional aerosol model, which means it represents the aerosol size distribution using a set of discrete size bins (typically 4 or 8). This allows for a more physically realistic representation of aerosol processes compared to bulk aerosol models.

### Key Processes in MOSAIC

-   **Nucleation**: Formation of new particles from the gas phase.
-   **Condensation/Evaporation**: Growth and shrinkage of existing particles.
-   **Coagulation**: Merging of smaller particles into larger ones.
-   **Aqueous-phase Chemistry**: Chemical reactions within cloud droplets.
-   **Thermodynamic Equilibrium**: Partitioning of semi-volatile species (like nitrate and ammonium) between the gas and aerosol phases.

### Aerosol Species

MOSAIC tracks a wide range of aerosol species, including:

-   Sulfate (SO4)
-   Nitrate (NO3)
-   Ammonium (NH4)
-   Black Carbon (BC)
-   Organic Carbon (OC)
-   Sea Salt (Na, Cl)

## The CBMZ-MOSAIC Coupling

In WRF-Chem, CBMZ and MOSAIC are coupled "online," meaning they exchange information at every model time step. This allows for a tight coupling between the gas and aerosol phases. For example, the condensation of sulfuric acid (H2SO4) gas, calculated by CBMZ, onto aerosol particles is handled by MOSAIC.

## Secondary Organic Aerosols (SOA)

A key consideration when using CBMZ/MOSAIC is the treatment of Secondary Organic Aerosols (SOA). While MOSAIC can simulate SOA, the standard CBMZ mechanism has a simplified representation of the gas-phase precursors to SOA. For studies where SOA is a primary focus, other chemical mechanisms, such as MOZART, may be more appropriate.

## Namelist Settings

To use CBMZ/MOSAIC, you will need to set the following in your `namelist.input`:

-   `chem_opt = 7` or `8`: CBMZ with MOSAIC (4 or 8 bins).
-   `aer_ra_feedback = .true.`: To allow aerosols to affect radiation.
-   `wet_scav_onoff = 1`: To turn on wet scavenging.

This configuration provides a robust framework for a wide range of air quality and atmospheric chemistry studies.
