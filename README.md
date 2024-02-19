# Fuzzy_wuzzy_matching

## Problem Statement
The task involves finding matching pairs of sales points from two different data sources.

## Data Provided
BASE A: A database from visits/surveys conducted periodically by Premise to establishments, needing to verify if the client has also visited them. This dataset contains 3,747 establishments with demographic information:

idRegistroEncuesta: Identifier of the measurement of the sales point (PDV)
idEstablecimiento: Unique identifier of the PDV
NombreEstablecimiento: Name of the PDV
NombrePropietario: Name of the owner or business name of the PDV
Direccion: Address of the PDV
Ciudad: City or Municipality of the PDV
GPS: Geographical coordinates of the PDV
StickerQR: QR sticker placed on the PDV for identification
Canal: Segmentation of the PDV's type of business
Localidad: Location of the PDV within a city or municipality
SubLocalidad: A smaller segmentation of the PDV's location within a locality
This database is in CSV format.

BASE B: A database from client visits to sales points for product sales, containing 6,039 establishments with demographic information, similar to BASE A but in XLSX (Excel) format.

## Objective
Perform dataset matching by utilizing the Levenshtein distance through Python's fuzz ratio function to compare location information across two datasets by its attributes.
