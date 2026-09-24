# code-An-open-source-GIS-workflow-for-optimal-rooftop-photovoltaic-panel-placement-in-urban-areas

This is the code for the workflow described in our paper on optimal rooftop photovoltaic panel placement. It takes a building address and a municipal DSM and returns an optimized panel layout, using only open-source tools and public data.

Paper is under review. Full citation will be added once it's out.

The script runs in QGIS through the Python Console. You give it an address (street type, street name, number), and it does everything else
#####################################################################################################
What it does:

- Finds the property and the building footprint
- Pulls the DSM points inside that footprint
- Interpolates them with IDW to get a roof surface
- Computes slope, aspect, and a weighted suitability layer
- Runs r.sun for 12 days and averages the result
- Multiplies irradiation by suitability to get an efficiency density map
- Places panels on a rotated grid, keeps only the ones that fit fully inside
- runs a greedy selection by density, no overlaps
- Classifies panels into three efficiency buckets
- Prints power and payback per bucket
#####################################################################################################
There's also a cleanup step that removes intermediate layers at the end so the project doesn't get cluttered.

######### Requirements #########

QGIS 3.34.12 or newer. GRASS and GDAL come bundled with QGIS, so you shouldn't need to install anything else. The script runs inside the QGIS Python Console.

######### Input layers #########

You need three layers loaded in the project, named exactly like this (or change the names at the top of the script):

- Adresses — property polygons
- Edifications — building footprints
- MDS — altimetric points, with an X, Y and Z field

The address layer needs three attributes: "TIPO_LOGRADOURO", "NOME_LOGRADOURO", "NUMERO_IMOVEL". If your data uses different names, edit the three constants at the top.

Everything must be in a metric CRS. We used SIRGAS 2000 / UTM 23S.

For Belo Horizonte the data is on BH Map (https://bhmap.pbh.gov.br).

######### Running it #########

Clone the repo, open QGIS, load the three layers, open the Python Console, paste the script. 

If you clip the DSM layer, ensure that the layer's points cover the entire extent of the addresses and buildings you intend to analyze.

At the bottom of the script there's a `raw_addresses` list. Put your addresses there as tuples:

```python
raw_addresses = [
    ("RUA", "ELM", 234),
    ("RUA", "BAKER", 221),
]

