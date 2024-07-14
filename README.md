# Karachi_NDVI
Sentinel-2 Derived NDVI Analysis of Karachi Using Google Colab

Introduction:
This study is about the analysis of Normalized Difference Vegetation Index (NDVI) derived from Sentinel-2 satellite imagery using Google Colab. The Sentinel-2 mission provides multispectral data that is crucial for monitoring vegetation health and changes over time. Leveraging Google Colab, a cloud-based Python environment, allows efficient processing and visualization of these satellite images without the need for local computational resources.

Methodology:
1. Data Retrieval: Sentinel-2 data is retrieved from the Sentinel Hub or Copernicus Open Access Hub and stored in Google Drive for easy access and organization.

2. Data Preprocessing: GeoTIFF files are accessed from Google Drive using Python libraries like rasterio. Bands relevant to NDVI calculation (typically NIR and Red bands) are selected and loaded into the Colab environment.

3. NDVI Calculation: NDVI is computed using the formula:
NDVI=NIR-Red / NIR+Red​.
This index helps in quantifying vegetation density and health.

4. Geospatial Visualization: The computed NDVI values are plotted on a geographic map using matplotlib. Geographic coordinates (latitude and longitude) are derived from the image metadata to accurately position the NDVI values spatially.

5. Analysis and Interpretation: The NDVI map provides insights into vegetation distribution, health, and changes over the observed area. This analysis can aid in environmental monitoring, agricultural planning, and ecosystem studies.

By utilizing Google Colab, this study ensures scalability, reproducibility, and accessibility of satellite image analysis, making it a valuable tool for remote sensing applications.



CODES:

Connect to gdrive:

# to connect gdrive

from google.colab import drive

drive.mount(‘/content/drive’)

install rsterio

!pip install rasterio matplotlib

Import Libraries

import os

import rasterio

import matplotlib.pyplot as plt

import numpy as np

from rasterio.plot import show

from rasterio.warp import transform

connect folder that has geotiff files or sentinel bands

# Define the path to your folder containing GeoTIFF files

geotiff_dir = ‘/content/drive/MyDrive/SENT_KHI24/’

# List all GeoTIFF files in the directory

geotiff_files = [f for f in os.listdir(geotiff_dir) if f.endswith(‘.tif’)]

print(“Processing and displaying GeoTIFF files:”)

for file in geotiff_files:

print(file)

Individual band displys

for file in geotiff_files:

file_path = os.path.join(geotiff_dir, file)

# Open the GeoTIFF file

with rasterio.open(file_path) as src:

# Read the first band

data = src.read(1)

# Plot the data

plt.figure(figsize=(10, 10))

plt.imshow(data, cmap=’gray’)

plt.colorbar()

plt.title(f’GeoTIFF Image — {file} — Band 1')

plt.show()

NDVI :

import rasterio

import matplotlib.pyplot as plt

import numpy as np

import os

from rasterio.plot import show

from rasterio.warp import transform

# Define the path to your folder containing GeoTIFF files

geotiff_dir = ‘/content/drive/MyDrive/SENT_KHI24/’

# Define the file names for NIR and Red bands

nir_band_file = ‘S2_Sindh_B8.tif’ # Adjust with your actual file names

red_band_file = ‘S2_Sindh_B4.tif’ # Adjust with your actual file names

# Ensure the files exist

if all(os.path.exists(os.path.join(geotiff_dir, band_file)) for band_file in [nir_band_file, red_band_file]):

# Read the NIR and Red bands

with rasterio.open(os.path.join(geotiff_dir, nir_band_file)) as src_nir:

nir = src_nir.read(1)

nir_transform = src_nir.transform

nir_crs = src_nir.crs

with rasterio.open(os.path.join(geotiff_dir, red_band_file)) as src_red:

red = src_red.read(1)

# Calculate NDVI

ndvi = (nir — red) / (nir + red)

# Get the boundaries of the image

nrows, ncols = ndvi.shape

xs = np.linspace(0, ncols — 1, ncols)

ys = np.linspace(0, nrows — 1, nrows)

x_coords, y_coords = np.meshgrid(xs, ys)

# Transform pixel coordinates to geographic coordinates

lon, lat = rasterio.transform.xy(nir_transform, y_coords, x_coords, offset=’center’)

lon = np.array(lon)

lat = np.array(lat)

# Plot NDVI with latitude and longitude

plt.figure(figsize=(10, 10))

plt.imshow(ndvi, cmap=’RdYlGn’, extent=(lon.min(), lon.max(), lat.min(), lat.max()))

plt.colorbar(label=’NDVI’)

plt.xlabel(‘Longitude’)

plt.ylabel(‘Latitude’)

plt.title(‘Normalized Difference Vegetation Index (NDVI)’)

plt.show()

else:

print(“One or more required band files are missing.”)






