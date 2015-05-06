# Hotspot-Analysis-in-R
Redo Hotspot Analysis in R


#### Marine Spatial Planning for Galapagos ####
# Hotspot/Coldspot Analysis converted from MatLab

## Load Packages
require(raster)

## Control File
# set model parameters
setwd("c:/Users/owashi/Documents/UCSB/SFG/Galapagos/Data/Galapagos-MSP-master")
datafolder <- "DATAFOLDER"
propfolder <- "PROPOSALS"
output <- "MSP RESULTS 2"
figurefolder <- "MSP FIGURES 2"

dir.create(output)
dir.create(datafolder)
dir.create(figurefolder)
dir.create(propfolder)

scale <- 500  # each cell equals XX meters
propnumbs <- 1  # which proposals to evaluate
noisl <- 1 # set this to 1 to knock out all islands
setdistance <- 0.08  # set this to the distance from shore that you want to include in the model, set to -1 to include all

mapfolder <- paste("MAPFOLDER", as.character(scale))  # NOT SURE ABOUT THIS



## Data
# use this section to pull in and name datafolder
# 1. baselayer
# 2. zoning
# 3. all kinds of data

# use this section to individually pull in and name each layer. Pain in the ass but clearer
#setwd("c:/Users/owashi/Documents/UCSB/SFG/Galapagos/Data/Galapagos-MSP-master/MAPFOLDER 500")
maps.blayer <- apply(read.csv(paste(mapfolder, "/basemap.csv", sep="")), 1, rev)  # baselayer of islands and ocean
maps.isls <- apply(read.csv(paste(mapfolder, "/islands.csv", sep="")), 1, rev)  # baselayer of islands and ocean
maps.bzones <- apply(read.csv(paste(mapfolder, "/rmg_zones_2006.csv", sep="")), 1, rev)  # current zoning scheme
maps.tvisit <- apply(read.csv(paste(mapfolder, "/tourism_visits.csv", sep="")), 1, rev)  # tourist visits
maps.upwell <- apply(read.csv(paste(mapfolder, "/upwelling.csv", sep="")), 1, rev)  # upwelling
maps.sstmin <- apply(read.csv(paste(mapfolder, "/sst_2003_2008_min.csv", sep="")), 1, rev)  # min SST
maps.sstmax <- apply(read.csv(paste(mapfolder, "/sst_2003_2008_max.csv", sep="")), 1, rev)  # max SST
maps.sstmean <- apply(read.csv(paste(mapfolder, "/sst_2003_2008_avg.csv", sep="")), 1, rev)  # mean SST
maps.sstsdv <- apply(read.csv(paste(mapfolder, "/sst_2003_2008_stdev.csv", sep="")), 1, rev)  # stdev of SST
maps.lobland <- apply(read.csv(paste(mapfolder, "/lobster_2008_catch_kriging.csv", sep="")), 1, rev)  # lobster catch (#'s)
maps.lobfish <- apply(read.csv(paste(mapfolder, "/lobster_2008_effort_kriging.csv", sep="")), 1, rev)  # lobster effort (diver days)
maps.lobcpue <- apply(read.csv(paste(mapfolder, "/lobster_2008_mdcpue_kriging.csv", sep="")), 1, rev)  # lobster CPUE (lobster per diver hours)
# maps.lobcpue <- apply(read.csv(paste(mapfolder, "/lobster2008mediancpue.csv", sep="")), 1, rev)  # lobster CPUE (lobster per diver hours)
# maps.lobland <- apply(read.csv(paste(mapfolder, "/lobster2008catch.csv", sep="")), 1, rev)  # lobster catch (#s)
# maps.lobfish <- apply(read.csv(paste(mapfolder, "/lobster2008effort.csv", sep="")), 1, rev)  # lobster effort (diver days)
maps.pepland <- apply(read.csv(paste(mapfolder, "/pepino_2008_catch_kriging.csv", sep="")), 1, rev)  # pepino catch (#s)
maps.pepfish <- apply(read.csv(paste(mapfolder, "/pepino_2008_effort_kriging.csv", sep="")), 1, rev)  # pepino diver hours)
maps.pepcpue <- apply(read.csv(paste(mapfolder, "/pepino_2008_mdcpue_kriging.csv", sep="")), 1, rev)  # median pepino/diver hours

maps.turtles <- apply(read.csv(paste(mapfolder, "/turtle_nests.csv", sep="")), 1, rev)  # sea turtle nesting sites
maps.fondos <- apply(read.csv(paste(mapfolder, "/fondos.csv", sep="")), 1, rev)  # bottom habitat types
maps.chloro <- apply(read.csv(paste(mapfolder, "/chl_a_avg_krig.csv", sep="")), 1, rev)  # average chlorophyll production
maps.endemic <- apply(read.csv(paste(mapfolder, "/endemic_galapagos_species_richness.csv", sep="")), 1, rev)  # endemic species richness
maps.indo_species <- apply(read.csv(paste(mapfolder, "/indo_pacific_species_richness.csv", sep="")), 1, rev)  # indo pacific species richness
maps.norsubtrop_species <- apply(read.csv("northern_sub_tropical_species_richness.csv"), rev)  # northern subtropical species richness
maps.nortemp_species <- apply(read.csv("northern_temperate_species_richness.csv"), rev)  # northern temperate species richness
maps.sousubtrop_species <- apply(read.csv("southern_sub_tropical_species_richness.csv"), rev)  # southern temperate species richness
maps.soutemp_species <- apply(read.csv("southern_temperate_species_richness.csv"), rev)  # southern temperate species richness
maps.all_species <- apply(read.csv("total_species_richness.csv"), rev)  # all species richness
maps.tropic_species <- apply(read.csv("tropical_species_richness.csv"), rev)  # tropical species richness
maps.distancetoshore <- apply(read.csv("distance_land.csv"), rev)  # distance to shore
maps.KrigedTourism <- apply(read.csv("tourism_visits_kriging.csv"), rev)  # kriged tourism data
maps.MacroZones <- apply(read.csv("macrozones.csv"), rev)  # kriged tourism data

data <- maps$fieldnames  # pull out names of each data layer


IslandLocations <- maps.isls
IslandLocations[IslandLocation==-9999)] = NaN
IslandLocations[isnan(IslandLocations)==0] = 0  # ISNAN HAS TO BE REPLACED WITH R FUNCTION THAT PUTS NaN VALUES IN AN ARRAY

mstack <- NaN[dim(maps.blayer,1),dim(maps.blayer,2), length(data)]  # create empty array to stack maps for easy manipulation
# fill in mstack with appropriate data
for d=1:length(data)
	





