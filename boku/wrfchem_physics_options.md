# WRF-Chem Physics Options

The choice of physics options in WRF-Chem is critical as they directly influence the transport, mixing, and removal of chemical species. This guide provides an overview of the key physics schemes and their relevance to atmospheric chemistry.

## Planetary Boundary Layer (PBL) Schemes

The PBL scheme determines how the lowest part of the atmosphere is mixed. This is crucial for chemistry as it controls the vertical distribution of pollutants emitted at the surface.

-   **`bl_pbl_physics`**: This namelist variable in the `&physics` section controls the PBL scheme.
-   **Common Options**:
    -   **YSU (Yonsei University) scheme**: A non-local scheme that tends to be more diffusive and can result in a deeper boundary layer. Often a good choice for convective situations.
    -   **MYJ (Mellor-Yamada-Janjic) scheme**: A local scheme that is less diffusive and can result in a shallower boundary layer.
-   **Relevance to Chemistry**: The depth of the PBL directly impacts the concentration of surface-emitted pollutants. A deeper PBL will lead to more dilution and lower surface concentrations.

## Microphysics Schemes

Microphysics schemes handle the formation and evolution of clouds and precipitation. In WRF-Chem, these schemes can also interact with aerosols.

-   **`mp_physics`**: This namelist variable controls the microphysics scheme.
-   **Aerosol-Aware Schemes**: Some microphysics schemes are "aerosol-aware," meaning they can use the model's aerosol fields to calculate cloud droplet number concentration. This allows for the simulation of aerosol indirect effects on clouds and radiation.
    -   **Morrison 2-moment scheme**: A popular choice for studying aerosol-cloud interactions.
    -   **Thompson scheme**: Another advanced scheme that can be coupled with the model's aerosol fields.
-   **Relevance to Chemistry**: Aerosol-aware microphysics is essential for studying the impact of pollution on weather and climate. These schemes also control wet deposition, a major sink for many chemical species.

## Cumulus Parameterizations

Cumulus schemes are used to represent deep convective clouds, which are important for vertical transport.

-   **`cu_physics`**: This namelist variable controls the cumulus scheme.
-   **Relevance to Chemistry**: Deep convection can rapidly transport pollutants from the boundary layer to the free troposphere, where they can have a longer lifetime and be transported over long distances. The choice of cumulus scheme can therefore have a significant impact on the large-scale distribution of chemical species. It is generally recommended to turn off the cumulus parameterization for grid resolutions below 5km (i.e. when convection is explicitly resolved).

The interactions between these physics schemes and the chemistry are complex. The best choice of schemes will depend on the specific research question and the region being studied.

## Physics-Chemistry Interactions

It is important to remember that the physics and chemistry components of WRF-Chem are not independent. The choices you make for the physics schemes can have a profound impact on the chemistry, and vice-versa.

-   **Aerosol-Radiation Feedback**: If you are using an aerosol-aware microphysics scheme, you can also turn on aerosol-radiation feedback (`aer_ra_feedback = .true.`). This allows the model's aerosol fields to affect the calculation of shortwave and longwave radiation, which can in turn affect the model's meteorology. This is a key process for studying the impact of aerosols on climate.
-   **Aerosol-Cloud Interactions**: As mentioned above, aerosol-aware microphysics schemes allow for the simulation of aerosol indirect effects. This is a major area of research in atmospheric science, and WRF-Chem is a powerful tool for studying these interactions.
-   **Wet Scavenging**: The microphysics and cumulus schemes control the removal of chemical species by precipitation (wet scavenging). The efficiency of this process depends on the type of precipitation and the chemical properties of the species.

When choosing your physics schemes, it is important to consider these interactions and to select a combination of schemes that is appropriate for your research goals.
