# WRF-Chem Chemistry Options: A Technical Guide

This guide provides a more technical look at the key chemistry-related namelist variables in the `&chem` section.

## Photolysis Options (`phot_opt`)

Photolysis rates are a critical input to the chemistry solver.

-   **Madronich scheme (`phot_opt = 1`)**: This scheme is computationally efficient, but it does not account for the effects of aerosols on photolysis rates.
-   **Fast-J scheme (`phot_opt = 2`)**: This scheme is more computationally expensive, but it explicitly calculates the impact of clouds and aerosols on photolysis rates. It is the recommended choice for most applications.
-   **TUV scheme (`phot_opt = 3`)**: The Tropospheric Ultraviolet and Visible (TUV) radiation model is another option that is similar in complexity to Fast-J.

**Best Practices**: For any study where aerosols are important, it is essential to use a photolysis scheme that accounts for aerosol-radiation interactions (i.e., Fast-J or TUV).

## Emissions Options

### Biogenic Emissions (`bio_emiss_opt`)

-   **MEGAN (Model of Emissions of Gases and Aerosols from Nature)**: MEGAN is a sophisticated model that calculates biogenic emissions based on land cover, weather, and other factors. It is the recommended choice for most applications.
    -   `bio_emiss_opt = 1`: MEGAN with online calculations.
    -   `bio_emiss_opt = 2`: MEGAN with offline calculations (reading from a file).

### Anthropogenic Emissions (`emiss_opt`)

-   The `emiss_opt` variable tells WRF-Chem which emissions inventory to use. The specific value of `emiss_opt` will depend on the inventory and the chemical mechanism. For example, `emiss_opt = 13` might correspond to the NEI 2011 inventory for the CBMZ mechanism.
-   **`emiss_inpt_opt`**: This option controls whether the emissions are read in as mass units or molar units.
-   **`io_style_emissions`**: This option controls the format of the emissions files.

**Best Practices**: It is crucial to ensure that your emissions inventory is compatible with your chosen chemical mechanism. The speciation of the emissions (i.e., how the total VOC emissions are split into the different species in the mechanism) is a critical step that is handled by the emissions pre-processor.

### Other Emissions

-   **Dust Emissions (`dust_opt`)**: WRF-Chem includes several options for simulating dust emissions, such as the GOCART-AFWA scheme (`dust_opt = 3`).
-   **Sea Salt Emissions (`seas_opt`)**: There are also several options for simulating sea salt emissions.

## Deposition Options

### Dry Deposition (`gas_drydep_opt`)

-   **Wesely scheme (`gas_drydep_opt = 1`)**: This is a resistance-based scheme where the deposition velocity is calculated as the inverse of the sum of three resistances: aerodynamic, quasi-laminar sublayer, and canopy.

### Wet Deposition (`wet_scav_onoff`)

-   Wet deposition (wet scavenging) is the removal of chemical species by precipitation. It is controlled by the `wet_scav_onoff` variable and is handled by the chosen microphysics scheme.
-   The efficiency of wet scavenging depends on the Henry's Law constant of the species, which determines its solubility in water.

By carefully selecting these chemistry options, you can create a more realistic and robust simulation of atmospheric composition.