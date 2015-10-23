 
 	How to use Kettle to create the OpenSextant Gazetteer
 	
 	OpenSextant use the Kettle ETL software  (officially called "Pentaho Data Integration Community Edition" ) to process and transform
 	the publicly available gazetteer data into a clean consistent form suitable to be ingested into a Solr repository and used by the OpenSextant geotagger.
 	
 	Here's how to do that: 
 
 	1) Get and install Kettle
 		 Get it from http://kettle.pentaho.com/
 		 Download and unzip anywhere handy.
 		(Developed/tested with versions "4.4.0-stable" )
 		NOTE: Kettle  "5.0.1.A-stable" introduced an intermittent issue reading the Excel files used for reference data. Avoid for now.
 		 
 	2) copy or rename build.local.properties to build.properties and edit:
 	 	a) set the "kettle.home" parameter to where you installed Kettle from step #1 above
 	 	b) set the proxy.host and proxy.port parameters if you are behind a firewall
 		c) set the "NGA_date" and "USGS_date" parameters (see build.properties for details)
 		d) (optional) modify the "kettle.options.jvm" setting to increase/reduce memory used in gazetteer processing. Setting this below about 1G will cause excessive processing times.
 		
 	3) do the build: ant publish-local
 	
 	 This will fetch the data from the two websites (NGA and USGS), unpack and rename the files and place them in their respective
 	 subdirectories of Gazetteer/GazetteerETL/GeoData/. It will then run the Kettle script (BuildMergedGazetteer.kjb) which will clean, transform 
 	 and output the finished gazetteer data in Gazetteer/GazetteerETL/GeoData/Merged/MergedGazeteer.txt (see Resources/UniversalGazetterModel.xlsx for the structure of this file)
 	 Depending on your machine this whole process can take up to 1.5 hrs.

 Structure of Gazetteer Project
 
 GazetteerETL 
 	BuildMergedGazetteer.kjb - the Kettle Job that does everything, it runs the Transformations below.
 	NGA to Universal.ktr - Kettle Transformation that cleans and transforms the NGA gazetteer data into GeoData/Merged/NGA.txt
 	USGS to Universal.ktr - Kettle Transformation that cleans and transforms the USGS gazetteer data into GeoData/Merged/USGS.txt
 	AdHoc to Universal.ktr - Kettle Transformation that cleans and transforms the user defined gazetteer data into GeoData/Merged/AdHoc.txt
 	EstimateBiases.ktr - Kettle Transformation that merges results of above three Transformations and adds some needed statistical measures to each gazetteer record.
 
 	GeoData - input (raw),intermediate and finished gazetteer data
		AdHoc - An Excel spreadsheet with few entries to patch a hole in the big official gazetteers. Also, an example of adding your own gazetteer data
 		NGA - The data from NGA GeoNames (http://earth-info.nga.mil/gns/html/namefiles.htm), the "World File"
 		USGS - The data from USGS GNIS (http://geonames.usgs.gov/domestic/download_data.htm) in three separate files:
 			a) The "National File" (could also use one of the single state files)
 			b) The "Government Units" Topical Gazetteer
 			c) The "All Names" Topical gazetteer
 		Merged - final (and almost final) gazetteer data
 				a) MergedGazetteer.tx - the final gazetteer data (transformed,merged, cleaned,deduped with estimated bias values)
 				b) MergedGazetteer_SMALL.txt - a small subset of the final gazetteer. Intended for testing. Contains only the "Basic" partition
 				c) clean.txt - this is the transformed,merged, cleaned and deduped gazetteer data (no bias or partition values)
 		transformed - contains intermediate stage data: data has been transformed to OpenSextant model but not yet cleaned nor bias values calculated
	
	lib - a couple of jars we use in the processing
	
 	Logs - directory where output logs go. Separate logs for duplicate and error(malformed/invalid) records. Duplicate and error records are not included in final gazetteer.
 			An additional log for records which have been labeled as SEARCH_ONLY is include for info purposes. These records are included in final gazetteer.
	
	Resources - data used in the cleaning, transformation and statistical estimation processes. See README in that directory for details.
  				
 