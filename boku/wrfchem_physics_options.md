# WRF-Chem Physics Options: A Technical Guide

The choice of physics options in WRF-Chem is a critical step that defines the behavior of the simulated atmosphere. This guide provides a more technical look at the key physics schemes and their interactions with the chemistry.

## Planetary Boundary Layer (PBL) Schemes (`bl_pbl_physics`)

The PBL scheme controls the turbulent mixing in the lowest part of the atmosphere.

-   **YSU (Yonsei University) scheme (`bl_pbl_physics = 1`)**: A first-order, non-local scheme with an explicit entrainment term. It is known for producing a well-mixed, deep boundary layer, which can be beneficial for simulating convective situations.
-   **MYJ (Mellor-Yamada-Janjic) scheme (`bl_pbl_physics = 2`)**: A one-and-a-half-order, local scheme that is less diffusive than YSU. It tends to produce a shallower, more stable boundary layer, which may be more appropriate for stable, nocturnal conditions.
-   **Other Options**: WRF offers a wide range of other PBL schemes, each with its own strengths and weaknesses.

**Best Practices**: The choice of PBL scheme is highly dependent on the research question. For air quality studies, it is often recommended to test several PBL schemes to assess the sensitivity of the results to the choice of scheme.

## Microphysics Schemes (`mp_physics`)

Microphysics schemes simulate the formation and evolution of clouds and precipitation.

-   **Morrison 2-moment scheme (`mp_physics = 10`)**: A sophisticated scheme that predicts the mass and number concentration of several hydrometeor species. It is a popular choice for studying aerosol-cloud interactions.
-   **Thompson scheme (`mp_physics = 8`)**: Another advanced scheme that is also well-suited for aerosol-cloud interaction studies. It includes a more detailed treatment of ice-phase processes.
-   **WDM6 (WRF Double-Moment 6-class) scheme (`mp_physics = 16`)**: A double-moment scheme that is computationally less expensive than Morrison or Thompson, but still provides a good representation of aerosol-cloud interactions.

**Best Practices**: For studies focusing on aerosol-cloud interactions, a double-moment scheme is essential. For other studies, a simpler single-moment scheme may be sufficient and will be computationally less expensive.

## Cumulus Parameterizations (`cu_physics`)

Cumulus schemes are used to represent sub-grid scale convective clouds.

-   **Kain-Fritsch scheme (`cu_physics = 1`)**: A popular and well-tested scheme that is known for producing realistic representations of convective systems.
-   **Grell-Freitas scheme (`cu_physics = 3`)**: An ensemble scheme that is also widely used.

**Best Practices**: Cumulus schemes should only be used for grid resolutions of 5km or coarser. For higher resolutions, convection should be explicitly resolved by the model, and the cumulus parameterization should be turned off (`cu_physics = 0`).

## Physics-Chemistry Interactions: A Deeper Look

The coupling between physics and chemistry is a key feature of WRF-Chem.

-   **Aerosol-Radiation Feedback (`aer_ra_feedback`)**: When this option is enabled, the model's aerosol fields are used in the radiation calculations. This allows for the simulation of the direct and semi-direct aerosol effects.
-   **Aqueous Chemistry (`chem_in_cloud`)**: This option enables the simulation of chemical reactions within cloud droplets. This is a major sink for some chemical species, such as sulfur dioxide (SO2).
-   **Wet Scavenging (`wet_scav_onoff`)**: This option controls the removal of chemical species by precipitation. The efficiency of this process is a function of the precipitation rate and the solubility of the chemical species (defined by its Henry's Law constant).

By carefully selecting the physics options, you can tailor your WRF-Chem simulation to your specific research goals and create a more realistic representation of the complex interactions between weather and chemistry.