# WRF-Chem Chemistry Options

Beyond selecting the main chemical mechanism (`chem_opt`), there are several other chemistry-related options in the `&chem` section of the `namelist.input` file that are crucial for a successful WRF-Chem simulation.

## Photolysis Options (`phot_opt`)

This option controls how the model calculates photolysis rates, which are the rates at which molecules are broken down by sunlight. This is a critical process in atmospheric chemistry, especially for the formation of ozone.

-   **`phot_opt = 1`**: Madronich photolysis scheme. A widely used scheme that is computationally efficient.
-   **`phot_opt = 2`**: Fast-J photolysis scheme. More computationally expensive than the Madronich scheme, but also more accurate, especially under cloudy conditions.

## Emissions Options

### Biogenic Emissions (`bio_emiss_opt`)

This option controls the emissions of volatile organic compounds (VOCs) from vegetation.

-   **`bio_emiss_opt = 1`**: MEGAN (Model of Emissions of Gases and Aerosols from Nature) with isoprene emissions.
-   **`bio_emiss_opt = 2`**: MEGAN with isoprene and monoterpene emissions.
-   **`bio_emiss_opt = 3`**: MEGAN with isoprene, monoterpenes, and sesquiterpenes.

### Anthropogenic Emissions (`emiss_opt`)

This option controls the emissions from human activities, such as industry and transportation. WRF-Chem can use a variety of anthropogenic emissions inventories, such as the National Emissions Inventory (NEI) for the United States, or global inventories like EDGAR or HTAP.

-   **Preparing Emissions**: Before you can use an emissions inventory in WRF-Chem, you will need to process it into the correct format. This is typically done using a pre-processor like `anthro_emiss` or the `prep_chem_sources` tool. These tools will map the emissions from their native grid to your WRF-Chem domain and speciate them for the chemical mechanism you are using.
-   **`emiss_opt`**: Once you have prepared your emissions files, you will set the `emiss_opt` in your `namelist.input` to tell WRF-Chem to use them.

### Other Emission Types

WRF-Chem can also simulate other types of emissions, such as:

-   **Biomass Burning Emissions**: Emissions from wildfires and prescribed burns can be included using a pre-processor to create time-varying emissions files.
-   **Volcanic Emissions**: WRF-Chem can be used to simulate the transport and chemistry of volcanic plumes.
-   **Dust and Sea Salt Emissions**: These are typically handled by online modules within WRF-Chem, rather than being read from a pre-processed file.

## Deposition Options

Deposition is the process by which chemical species are removed from the atmosphere and deposited onto the Earth's surface.

### Gas-Phase Dry Deposition (`gas_drydep_opt`)

-   **`gas_drydep_opt = 1`**: Wesely dry deposition scheme. A widely used scheme that calculates the deposition velocity based on the land use type and meteorological conditions.

### Aerosol Dry Deposition (`aer_drydep_opt`)

-   **`aer_drydep_opt = 1`**: A simple scheme that is often used with the Wesely gas-phase scheme.

### Wet Deposition (`wet_dep_opt`)

Wet deposition is the removal of chemical species by precipitation. This is handled by the chosen microphysics scheme (`mp_physics`) and is not set as a separate option in the `&chem` section.

The choice of these chemistry options will depend on the specific goals of your simulation. For example, if you are studying the impact of biogenic emissions on ozone formation, you will need to use a detailed biogenic emissions model like MEGAN.
