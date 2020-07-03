# LPJmL Tools 

[[_TOC_]]

This is a repository for configuration files, and pre- and postprocessing tools for LPJmL runs within WUR. For the model itself, check out the official LPJmL repository maintained by PIK (see below).

## What is LPJmL?

The model LPJmL ("Lund-Potsdam-Jena managed Land") is designed to simulate vegetation composition and distribution as well as stocks and land-atmosphere exchange flows of carbon and water, for both natural and agricultural ecosystems. Using a combination of plant physiological relations, generalized empirically established functions and plant trait parameters, it simulates processes such as photosynthesis, plant growth, maintenance and regeneration losses, fire disturbance, soil moisture, runoff, evapotranspiration, irrigation and vegetation structure.

LPJmL is currently the only DGVM that has dynamic land use fully incorporated at the global scale and also simulates the production of woody and herbaceous short-rotation bioenergy plantations and the terrestrial hydrology. It differs from other models in the wider field by computing carbon, nitorgen and water flows explicitly: most other macro-hydrological models lack the important vegetation structural and physiological responses that influence the water cycle, while most other vegetation models lack the advanced consistent water balance of LPJmL, or are not global in scale.

![alt text](https://www.pik-potsdam.de/research/projects/activities/biosphere-water-modelling/images/LPJmL_gridcell_blanc2.png "Each grid cell in LPJmL simulations can consist of indivudual land use types or a mosaic of variable fractions of different agricultural lands and natural vegetation. Original image: PIK")

## Links

LPJmL version 4.0 as described by [Schaphoff et al. 2018a](http://dx.doi.org/10.5194/gmd-2017-145) and evaluated by [Schaphoff et al. 2018b](http://dx.doi.org/10.5194/gmd-2017-146) is freely available under the [AGPLv3 license](https://www.gnu.org/licenses/agpl-3.0.en.html). The code can be downloaded from github. 

* [Official LPJmL web page](https://www.pik-potsdam.de/research/projects/activities/biosphere-water-modelling/lpjml/lpjml)
* [LPJmL GitHub repository](https://github.com/PIK-LPJmL/LPJmL)
* [LPJmL Wiki](https://github.com/PIK-LPJmL/LPJmL/wiki) with practical information on how to run the model

## Getting started

To clone this repository, type the following in a suitable working directory:

```
$ git clone git@git.wur.nl:danke010/lpjml_tools.git
```

The URL for cloning can also be found in the [main repository page in GitLab](https://git.wur.nl/danke010/lpjml_tools).

* [Some information on getting started with Git within WUR](https://wiki.anunna.wur.nl/index.php/Manual_GitLab)