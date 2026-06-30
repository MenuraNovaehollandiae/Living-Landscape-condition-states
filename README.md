# Living Landscape mallee condition-state maps

This repository contains an interactive web map showing vegetation condition states across the Living Landscape study region in the South Australian mallee.

The mapped condition states were developed from expert-elicited State-and-Transition Models (STMs) and represent combinations of overstorey and understorey condition. States range from reference vegetation through to modified understorey, sparse or modified overstorey, and collapsed overstorey condition.

## Overview

The condition-state map was derived by combining three spatial inputs:

1. a vegetation-type map for the Living Landscape study region;
2. a tree canopy / overstorey condition map; and
3. an understorey condition map based on a grazing pressure index.

These layers were combined using a rule-based matrix that assigns each pixel to an STM condition-state code according to vegetation type, overstorey condition, and understorey condition.

The final web map is intended as a decision-support product for visualising broad patterns in mallee vegetation condition across the study region.

## Study region

The map covers the Living Landscape case study region in the South Australian mallee, including the Calperum landscape where expert elicitation workshops were undertaken.

## Methods summary

### Vegetation type

The STM condition-state map uses a vegetation-type map developed for the Living Landscape study region. The rule-based condition-state assignment focused on mallee vegetation types considered to occur across the case study region, including:

- Shrubby Mallee
- Triodia Mallee
- Derived Grassland

Mesic mallee and chenopod mallee were not considered to occur across the case study region for this mapping application.

### Understorey condition

Understorey condition was mapped using a grazing pressure index derived from pastoral dam data. Dam data were sourced from Gillespie (2025), which provided an inventory of pastoral dams and an assessment of their capacity to retain water. The original dam-water assessment was derived from Sentinel-2 imagery using NDWI to estimate water persistence over a 10-year period.

For each dam, grazing pressure was estimated from water availability and recovery since decommissioning. Water availability was calculated as the proportion of study-period days in which standing water was detected. For decommissioned dams, this value was reduced using a recovery multiplier based on years since closure and an assumed 30-year recovery timeframe.

A distance-decay surface was then generated around each dam using an exponential decay function scaled so that grazing pressure declined to approximately 0.01 at 8 km from the dam. Dam-specific decay rasters were combined using a cell-wise maximum, so that each pixel was assigned the highest grazing pressure value from any dam.

The continuous grazing pressure surface was then reclassified into four ordinal understorey condition classes:

| Understorey class | Description | GPI threshold |
|---:|---|---|
| 1 | Collapsed | ≥ 0.239 |
| 2 | Highly modified | 0.080 to < 0.239 |
| 3 | Modified | 0.004 to < 0.080 |
| 4 | Reference | < 0.004 |

These thresholds were calibrated using the top quartile of dams with the highest and most persistent water availability. Mean GPI values were extracted at 100 m, 2 km, and 6 km from dam centroids to represent collapsed, highly modified, modified, and reference understorey condition classes.

### Overstorey / canopy condition

Tree canopy was initially mapped as a binary treed / cleared class using a Random Forest classifier implemented in Google Earth Engine. The classifier used 200 trees and the same predictor variables as the vegetation-type classification. Reference data comprised manually digitised canopy points, and cross-validation yielded a user’s accuracy of 77% for the treed class.

To derive canopy-cover classes relevant to the STM, a tree-pixel density metric was calculated using an 11 × 11 moving window. The resulting canopy-density classes were calibrated against high-resolution 30 cm Bing Maps imagery processed in ImageJ.

Five canopy classes were used:

| Canopy class | Description | Approximate canopy cover |
|---:|---|---|
| 1 | No trees | 0 pixels |
| 2 | Sparse canopy | ≤ 5% cover |
| 3 | Open canopy | 6–15% cover |
| 4 | Moderate canopy | 16–20% cover |
| 5 | Dense canopy | > 20% cover |

Post-classification cleaning removed isolated treed pixels with one or fewer treed neighbours in a 3 × 3 window, followed by a 3 × 3 mode filter.

## STM condition-state assignment

Final STM condition-state codes were assigned using a rule-based matrix that combined vegetation type, canopy condition class, and understorey condition class.

For Triodia Mallee, canopy class 5 was treated as reference overstorey, classes 3–4 as modified overstorey, class 2 as sparse or absent overstorey, and class 1 as collapsed overstorey.

For Shrubby Mallee, canopy classes 4–5 were treated as reference overstorey, class 3 as modified overstorey, class 2 as sparse or absent overstorey, and class 1 as collapsed overstorey.

Understorey classes were interpreted as:

| Understorey class | Condition |
|---:|---|
| 4 | Reference |
| 3 | Modified |
| 2 | Highly modified |
| 1 | Collapsed |

Derived Grassland was assigned to STM code 9, representing collapsed overstorey and highly modified understorey.

Several theoretically possible combinations involving collapsed understorey were included in the STM matrix but were not expected to occur across the mapped case study region.

## Condition-state raster values

The STM condition-state raster uses integer values to represent mapped vegetation condition states. Raster values correspond to the following condition-state classes:

| Raster value | Condition state |
|---:|---|
| 1 | Reference |
| 2 | Modified overstorey, reference understorey |
| 3 | Reference overstorey, modified understorey |
| 4 | Modified overstorey, modified understorey |
| 5 | Sparse / absent overstorey, modified understorey |
| 6 | Reference overstorey, highly modified understorey |
| 7 | Modified overstorey, highly modified understorey |
| 8 | Sparse / absent overstorey, highly modified understorey |
| 9 | Collapsed overstorey, highly modified understorey |
| 10 | Reference overstorey, collapsed understorey — not expected to occur in the mapped study region |
| 11 | Modified overstorey, collapsed understorey — not expected to occur in the mapped study region |
| 12 | Sparse / absent overstorey, collapsed understorey — not expected to occur in the mapped study region |
| 13 | Collapsed overstorey and understorey — crop; not expected to occur in the mapped study region |
| 14 | Collapsed overstorey and understorey — eucalyptus oil plantation; not expected to occur in the mapped study region |
| 15 | Diverse Triodia grassland / shrubland with resprouting mallee |
| 16 | Depauperate Triodia grassland / shrubland with resprouting mallee |
| 17 | Depauperate Triodia / shrubby mallee woodland |

Values 1–14 represent the main overstorey–understorey condition matrix. Values 15–17 represent additional Triodia / shrubland and resprouting mallee states included in the STM framework.

## How to use the map

Open the GitHub Pages website associated with this repository to view the interactive map. The map can be used to explore spatial patterns in vegetation condition across the Living Landscape study region.

The map is intended as a broad-scale decision-support product. It should be interpreted alongside field validation, local ecological knowledge, and the assumptions of the underlying State-and-Transition Models.

## Suggested citation

Maisey, A. C. et al. 2026. *Living Landscape mallee condition-state maps*. Interactive web map. GitHub repository.

**********Update this citation with a DOI when available**********

## Acknowledgement

This project was supported by funding from the Australian Government’s National Environmental Science Program through the Resilient Landscapes Hub.

We acknowledge the Traditional Owners of Country throughout Australia and their continuing connection to land, sea and community, and pay our respects to Elders past and present.

## Licence

Unless otherwise stated, written content and map outputs in this repository are provided under a Creative Commons Attribution 4.0 International licence.

Basemap imagery and third-party spatial data remain subject to their original licences and terms of use.
