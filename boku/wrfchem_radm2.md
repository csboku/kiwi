# RADM2 Chemical Mechanism

The Regional Acid Deposition Model 2 (RADM2) is a chemical mechanism that is simpler and computationally less expensive than more comprehensive mechanisms like MOZART. It is a good choice for simulations where detailed tropospheric chemistry is not the primary focus, or for users who are new to WRF-Chem.

## Key Features

-   **Simplified Chemistry**: RADM2 includes a condensed set of chemical reactions for ozone and its precursors. It is less detailed in its representation of VOC chemistry compared to MOZART.
-   **Lower Computational Cost**: Due to its simplicity, RADM2 runs much faster than more complex mechanisms.
-   **Good for Air Quality Studies**: It is well-suited for regional air quality studies focusing on ozone and acid deposition.

## Use Cases

-   **Good for Beginners**: Due to its simplicity and low computational cost, RADM2 is an excellent choice for users who are new to WRF-Chem.
-   **Regional Ozone Studies**: It is well-suited for regional air quality studies where the primary focus is on ozone and its precursors, and where detailed VOC chemistry is not essential.
-   **Computationally Limited Studies**: If you have limited computational resources, RADM2 provides a good balance between chemical detail and computational cost.

## WRF-Chem `chem_opt`

To use the RADM2 mechanism in WRF-Chem, set the following in your `namelist.input` file:

-   `chem_opt = 2`: RADM2 chemical mechanism.

## Considerations

-   **VOCs**: The lumping of VOC species in RADM2 may not be suitable for all research questions. If your study focuses on specific VOCs, a more detailed mechanism might be necessary.
-   **Aerosols**: RADM2 is often used with simpler aerosol schemes.

For more detailed information, refer to the official WRF-Chem User's Guide and the original scientific papers describing the RADM2 mechanism.
