This directory "Resources" conatins the reference data, mappings and other information used during the processing of the
source gazetteers into the form used by OpenSextant.

README.txt - this file
UniversalGazetterModel.xlsx describes the OpenSextant gazetteer model (fields, descriptions and valid values)  
minedScores.xlsx - contains PLACE_NAME_BIAS scored from external process which overide estimated score. Used but currently contains only trivial values.
NamePatternWeights.xlsx - the category patterns and the weights assigned to them. Used is estimating the PLACE_NAME_BIAS
USGS GNIS to Universal FeatureCode.xlsx - mapping from the USGS feature codes to the OpenSextant codes
USGSCountryAdjusts.xlsx - adjustments for tyhe few records from USGS which are not country code "US"
vocab.xlsx - categorized vocabualry and affices used in estimating the PLACE_NAME_BIAS value
wellKnownPlaces.xlsx - list of well known cites, which is used in estimating the PLACE_ID_BIAS value
wordstats.txt - large set of vocabulary with case (upper,lower,initial, mixed) statitsics, used in estimating the PLACE_NAME_BIAS value
wordstats_README.txt - details of the wordstats.txt contents

country-names-2013.csv - legacy dataset provided with gazetter data but not maintained
feature-metadata-2013.csv- legacy dataset provided with gazetter data but not maintained