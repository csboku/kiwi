# Analyzing WRF-Chem Output with R: A Technical Guide

R provides a powerful and flexible ecosystem for analyzing and visualizing WRF-Chem output. This guide introduces a modern workflow using the `ncdf4`, `metR`, and `openair` packages.

## Core Packages

-   **`ncdf4`**: The fundamental package for NetCDF file manipulation.
-   **`metR`**: Extends `ggplot2` for meteorological data, providing tools for visualization and analysis.
-   **`openair`**: A comprehensive package for air quality and atmospheric composition analysis.

## Basic Workflow: Surface Maps

The fundamental workflow involves opening the data, extracting variables, and plotting them.

```R
# Load the required libraries
library(ncdf4)
library(ggplot2)
library(metR)

# Open the wrfout file
nc_file <- nc_open("wrfout_d01_...")

# Read the data
ozone <- ncvar_get(nc_file, "o3", start = c(1, 1, 1, 1), count = c(-1, -1, 1, 1))
lat <- ncvar_get(nc_file, "XLAT")
lon <- ncvar_get(nc_file, "XLONG")
nc_close(nc_file)

# Create a data frame and plot
df <- data.frame(lon = as.vector(lon), lat = as.vector(lat), ozone = as.vector(ozone) * 1e9)
ggplot(df, aes(x = lon, y = lat, fill = ozone)) +
  geom_raster() +
  scale_fill_viridis_c() +
  labs(title = "Surface Ozone", fill = "Ozone (ppb)") +
  theme_minimal()
```

## Advanced Example: Time Series Analysis with `openair`

The `openair` package is excellent for time series analysis.

```R
# Load the required libraries
library(ncdf4)
library(openair)
library(dplyr)

# --- Function to find the nearest grid cell ---
find_nearest_grid_cell <- function(lon_wrf, lat_wrf, target_lon, target_lat) {
  dist <- sqrt((lon_wrf - target_lon)^2 + (lat_wrf - target_lat)^2)
  which(dist == min(dist), arr.ind = TRUE)
}

# --- Main script ---
# Target location
target_lon <- -97.5
target_lat <- 35.5

# Open the wrfout file
nc_file <- nc_open("wrfout_d01_...")

# Read lat/lon and find the nearest grid cell
lat <- ncvar_get(nc_file, "XLAT")
lon <- ncvar_get(nc_file, "XLONG")
grid_cell <- find_nearest_grid_cell(lon, lat, target_lon, target_lat)

# Read the time series of ozone for the nearest grid cell
ozone_ts <- ncvar_get(nc_file, "o3",
                      start = c(grid_cell[2], grid_cell[1], 1, 1),
                      count = c(1, 1, 1, -1))

# Read the time variable
time_char <- ncvar_get(nc_file, "Times")
time <- as.POSIXct(apply(time_char, 1, paste, collapse = ""), format = "%Y-%m-%d_%H:%M:%S")

nc_close(nc_file)

# Create a data frame for openair
df_ts <- data.frame(
  date = time,
  o3 = as.vector(ozone_ts) * 1e9 # Convert to ppb
)

# Create a time series plot
timePlot(df_ts, pollutant = "o3", ylab = "Ozone (ppb)")
```

This workflow provides a modern and powerful approach to analyzing WRF-Chem data with R. By leveraging these packages, you can create efficient, scalable, and reproducible analysis pipelines.