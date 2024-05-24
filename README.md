

# Integrated Approach to Global Land Use and Land Cover Reference Data Harmonization
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].



***
## Content
1. [Introduction](#introduction)
2. [Objectives](#objectives)
3. [Data Collection](#datacollection)
4. [Data files and metadata](#datafilesmeta)
5. [Workflow](#workflow)
6. [Results](#results)
7. [Acknowledgments](#ack)
9. [References](#references)


***
## Introduction <a name="introduction"></a>
This document outlines the creation of a global inventory of reference samples and Earth Observation (EO) / gridded datasets for the Global Pasture Watch (GPW) initiative. This inventory supports the training and validation of machine-learning models for GPW grassland mapping. This documentation outlines methodology, data sources, workflow, and results.

<b>Keywords:</b> Grassland, Land Use, Land Cover, Gridded Datasets, Harmonization.

## Objectives <a name="objectives"></a>
  - Create a global inventory of existing reference samples for land use and land cover (LULC).
  - Compile global EO / gridded datasets that capture LULC classes and harmonize them to match the GPW classes.
  - Develop automated scripts for data harmonization and integration.

## Data Collection <a name="datacollection"></a>

The following datasets were integrated:
- [Land Change Monitoring, Assessment, and Projection (LCMap) (U.S.)](https://www.usgs.gov/special-topics/lcmap/lcmap-conus-reference-data)
- [MapBiomas samples (Brazil)](https://zenodo.org/record/5136666#.ZEE08HpBwXc)
- [Land Use/Land Cover Area Frame Survey LUCAS samples (Europe)](https://data.jrc.ec.europa.eu/dataset/f85907ae-d123-471f-a44a-8cca993485a2)
- [GeoWiki C-GLOPS training dataset (Global)](https://data.jrc.ec.europa.eu/dataset/f85907ae-d123-471f-a44a-8cca993485a2)
- [PREDICTS database samples (Global)](https://data.nhm.ac.uk/dataset/the-2016-release-of-the-predicts-database)
- [WorldCereal samples (Global)](https://zenodo.org/communities/worldcereal-rdm?page=1&size=20)
- [DynamicWorld samples (Global)](https://doi.pangaea.de/10.1594/PANGAEA.933475)
- [CropHarvest samples (Global)](https://github.com/nasaharvest/cropharvest)
- [EuroCrops samples (Global)](https://github.com/maja601/EuroCrops)
- [Global Land Cover Mapping and Estimation (GLanCE) samples (Global)](https://beta.source.coop/boston-university/bu-glance/)



| Datasets|Spatial distribuition|Time period| Number of individual samples|
| :---         | :---:           | :---:          | :---:     |
| WorldCereal   | Global     | 2016-2021    | 38,267,911  |
| GLanCE     | Global       | 1985-2021      | 31,061,694  |    
| EuroCrops   | Europe     | 2015-2022    | 14,742,648  |
| GeoWiki G-GLOPS training dataset     | Global       | 2021      | 11,394,623  | 
| MapBiomas Brazil   | Brazil     | 1985-2021    | 3,234,370  |
| Land Use and Land Cover Area Frame Survey (LUCAS)| Europe       | 2006-2018      | 1,351,293  | 
| Dynamic World | Global     | 2019-2020    | 1,249,983  |
| Land Change Monitoring,Assessment, and Projection (LCMap)     | U.S. (CONUS) | 1984-2018      | 874,836  | 
| GeoWiki 2012   | Global     | 2011-2012    | 151,942  |
| PREDICTS     | Global       | 1984-2013      | 16,627  | 
| Crop Harvest (NASA)| Global       |       | 9,714  |
|<b> Total </b>|||102,355,642|

## Data files and metadata<a name="harmonsiation"></a>
[Link to files and metadata](https://drive.google.com/drive/folders/1JNncgbxoW3-OeU-QCkRNnPGC6T-0efe_?usp=sharing)


## Workflow <a name="workflow"></a>
![g1924](figures/workflow-dataprocessing.png)

<b>Harmonization Process</b>

We harmonized global reference samples and EO/gridded datasets to align with GPW classes, optimizing their integration into the GPW machine-learning workflow. 
We considered reference samples derived by visual interpretation with spatial support of at least 30 m (Landsat and Sentinel), that could represent LULC classes for a point or region. 

Each dataset was processed using automated Python scripts to download vector files and convert the original LULC classes into the following GPW classes:
  
  0. 	Other land cover
  1. Natural and Semi-natural grassland
  2. Cultivated grassland
  3. Crops and other related agricultural practices

We empirically assigned a weight to each sample based on the original dataset's class description, reflecting the level of mixture within the class. The weights range from 1 (Low) to 3 (High), with higher weights indicating greater mixture. Samples with low mixture levels are more accurate and effective for differentiating typologies and for validation purposes.

The harmonized dataset includes these columns:

| Attribute Name | Definition                                                |
| -------------- | ----------------------------------------------------------- |
| dataset_name     | The name of original dataset LULC|
| reference_year | The reference year of samples the orginal dataset.|
| original_lulc_class      | Name classe land use and land cover the original dataset. |
| gpw_lulc_class      | Name classe land use and land cover the Global Pasture Watch project. |
| sample_weight      | The sample weight   |

<p></p>
<b>Reclassification Process</b>
<p></p>
We developed a specifically tailored grassland ontology. This ontology serves as a reference system for classifying different grassland types, facilitating the harmonization of reference samples obtained from various sources.
<p></p>
Our ontology likely focuses on differentiating between two primary grassland classes relevant to the project: cultivated grasslands (1) and natural or semi-natural grassland (2).
<p></p>
Using the ontology as a guide, we reclassified samples from original datasets. Here's how we approached the different scenarios:

- Broader Definitions: For datasets with a single, broad classification level (e.g., grassland or herbaceous vegetation), we relied on the description provided within the dataset's metadata to determine the most appropriate mapping to our ontology's classes. In those cases, the level of mixture of different grassland typologies was higher, meaning the samples might not effectively differentiate between cultivated and natural grasslands.

- Multiple Classification Levels: Some datasets might have more detailed classifications. For example, a class named "Temporary grass crops" would likely be reclassified as "cultivated grassland (class 1)" based on the ontology's definition; while a class like "Savannah" would be reclassified as "natural grassland (class 2)".



<b>Look-up tables</b> 

Datasets lookup tables https://docs.google.com/spreadsheets/d/1i9mQefIW_eOJNW29mf9EqB6BcZr1UAM0HbP9iCE1icI/edit?usp=sharing



<b>Quality Assurance Analysis</b>

Our Quality Assurance (QA) analysis focused on data accuracy and consistency. We ensured the integrity of our dataset through the following approach:

- Duplicate Removal: We identified and excluded duplicate rows. This involved checking for rows with identical coordinates and corresponding years. These duplicates likely represent redundant entries within the original dataset or within different datasets.

- Outlier Detection: We screened for outliers by identifying data points with values exceeding the geographic boundaries of the original source's study area. Such values could indicate errors in data collection or processing within the original dataset.
By eliminating duplicates and outliers, we ensure that the training and validation processes rely on a clean and accurate dataset.


## RESULTS<a name="result"></a>
The final harmonized reference samples and documented processes are uploaded to Google Drive and GitHub for sharing and reproducibility.

[Link to the harmonized data on Google Drive](https://drive.google.com/file/d/1yDFaacsHN6VnEI0H_fckgA5WpoP2Q0F-/view?usp=drive_link)

[Link to the Quality Assurence data on Google Drive](https://drive.google.com/file/d/1ZnmaCNXUIGDMyYqdVyPIUEGXCw_QzN6p/view?usp=drive_link)


## ACKNOWLEDGMENTS<a name="ack"></a>
The development of this global inventory of reference samples and EO/gridded datasets relied on valuable contributions from various sources. We would like to express our sincere gratitude to the creators and maintainers of all datasets used in this project.



## Reference<a name="reference"></a>

- Van Tricht, K. et al. Worldcereal: a dynamic open-source system for global-scale, seasonal, and reproducible crop and irrigation mapping. Earth Syst. Sci. Data 15, 5491–5515, 10.5194/essd-15-5491-2023 (2023)

- Schneider, M., Schelte, T., Schmitz, F. & Körner, M. Eurocrops: The largest harmonized open crop dataset across the european union. Sci. Data 10, 612, 10.1038/s41597-023-02517-0 (2023)

- Souza, C. M. et al. Reconstructing Three Decades of Land Use and Land Cover Changes in Brazilian Biomes with Landsat Archive and Earth Engine. Remote. Sens. 12, 2735, 10.3390/rs12172735 (2020)
Stanimirova, R. et al. A global land cover training dataset from 1984 to 2020. Sci. Data 10, 879 (2023)

- d’Andrimont, R. et al. Harmonised lucas in-situ land cover and use database for field surveys from 2006 to 2018 in the european union. Sci. data 7, 352, 10.1038/s41597-019-0340-y (2020)

- Stehman, S. V., Pengra, B. W., Horton, J. A. & Wellington, D. F. Validation of the us geological survey’s land change monitoring, assessment and projection (lcmap) collection 1.0 annual land cover products 1985–2017. Remot Sensing environment 265, 112646, 10.1016/j.rse.2021.112646 (2021).

- Tsendbazar, N. et al. Product validation report (d12-pvr) v 1.1 (2021)




[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
