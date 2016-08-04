##OpenSextant Gazetteer 


 The OpenSextant Gazetteer is a collection of world-wide place name data. It's used by the other OpenSextant projects, like the OpenSextant geotagger but it is also useful for any project that needs clean, consistent place name data.

###Released Data
Most projects will just want the finished data ([Latest Release](https://github.com/OpenSextant/Gazetteer/releases/latest))

###Do it yourself
OpenSextant use [Kettle ETL](http://kettle.pentaho.com) to process and transform the publicly available gazetteer data into a clean consistent form used by the OpenSextant geotagger. 

* Get and install Kettle
  * Get it from http://kettle.pentaho.com (Tested with versions "4.4.0-stable" and 6.1 )  
NOTE: Kettle  "5.0.1.A-stable" introduced an intermittent issue reading the Excel files used for reference data. Avoid for now. Also 6.0.x introduced a bug for the User Defined Java step. Likewise avoid for now.
Version 6.1 generates a large number of inscrutable but harmless warning messages in the log:
   *Warning: The configuration parameter [org] is not supported by the default configuration builder for scheme: sftp*
* Configure
  * copy or rename build.local.properties to build.properties and edit:
     * set the "kettle.home" parameter to where you installed Kettle from step #1 above
     * set the proxy.host and proxy.port parameters if you are behind a firewall
     * set the "NGA_date" and "USGS_date" parameters (see build.properties for details)
     * (optional) modify the "kettle.options.jvm" setting to increase/reduce memory used in gazetteer processing. Setting this below about 1G will cause excessive processing times.  
* Do the build
  * run `ant`  
This will fetch the data from the two websites (NGA and USGS) and run the Kettle processes which will clean, transform and zip the finished gazetteer data into the build directory. 
  

Depending on your machine this whole process can take up to 1.5 hrs.

 
