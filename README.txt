 
 	How to use Kettle to create the OpenSextant Gazetteer
 	
 	OpenSextant use the Kettle ETL software  (officially called "Pentaho Data Integration Community Edition" ) to process and transform
 	the publicly available gazetteer data into a clean consistent form suitable to be ingested into a Solr repository and used by the OpenSextant geotagger.
 	
 	Here's how to do that: 
 
 	1) Get and install Kettle
 		 Get it from http://kettle.pentaho.com/
 		 Download and unzip anywhere handy.
 		(Developed/tested with versions "4.4.0-stable" )
 		NOTE: Kettle  "5.0.1.A-stable" introduced an intermittent issue reading the Excel files used for reference data. Avoid for now.
 		 
 	 You may want to give Kettle some more memory. To do so, edit <KETTLE_HOME>/Spoon.bat (or spoon.sh, whichever you intend to use) to increase the Java heap space
 			In either file, change 
 				PENTAHO_DI_JAVA_OPTIONS="-Xmx512m"
 			to
 				PENTAHO_DI_JAVA_OPTIONS="-Xmx1200m"	
 		
 	2) copy or rename build.local.properties to build.properties and edit:
 	 	a) set the "kettle.home" parameter to where you installed Kettle from step #1 above
 	 	b) set the proxy.host and proxy.port parameters if you are behind a firewall
 		c) set the "NGA_date" and "USGS_date" parameters (see build.properties for details)
 		
 	3) do the build: ant
 	
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
 
 	GeoData - input (raw) gazetteer data
		AdHoc - An Excel spreadsheet with few entries to patch a hole in the big official gazetteers. Also, an example of adding your own gazetteer data
 		NGA - The data from NGA GeoNames (http://earth-info.nga.mil/gns/html/namefiles.htm), the "World File"
 		USGS - The data from USGS GNIS (http://geonames.usgs.gov/domestic/download_data.htm) in three separate files:
 			a) The "National File" (could also use one of the single state files)
 			b) The "Government Units" Topical Gazetteer
 			c) The "All Names" Topical gazetteer
	
	lib - a couple of jars we use in the processing
	
 	Logs - directory where output logs go. Each of the data transformation steps (NGA,USGS and AdHoc) will create logs for any duplicate and error(malformed/invalid) records found.
	
	Resources - data used in the cleaning, transformation and statistical estimation processes
  				
 