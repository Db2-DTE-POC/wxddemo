# Datasets

There are three datasets that have been loaded into watsonx.data system for you to use while exploring the features of the product.

   * Great Outdoors Company warehouse data
   * Airline On-Time performance
   * Taxi fares

If you want to load you own data, check out the following website for a variety of public data sets that you can use.

* <a href="https://github.com/awesomedata/awesome-public-datasets" taget="_blank">Awesome Public Datasets</a>

## Data Location

The data files can be found in the <code style="color:blue;font-size:medium;">/sampledata</code> directory. Underneath this directory you will find datasets in three different formats:

* Parquet - Data that has been formatted in Parquet format that can be loaded directly into Hive and queried by watsonx.data.
* Relational - Data that is in a delimited format that can be loaded into Db2 or PostgreSQL databases.
* CSV - Comma separated values that can be converted to multiple formats or used by watsonx.data.

Within the Parquet and Relational directories are SQL statements that can be used to catalog and load the data into the different systems. 

## Loading your own data

You can use a browser or link to an external file repository (i.e., Box) and download data directly into the virtual machine. If you have data in your workstation that you want to load into the image, use the following steps. **Note**: You cannot import customer data nor any data that has restrictions associated with its use. Any use of private data is in violation of the terms and conditions of using this image.

Use a terminal shell to copy the source data into a temporary location in the watsonx userid location.

```
scp ~/Downloads/myfile.csv watsonx@192.168.252.2:/home/watsonx/Downloads
```

This command will copy the `myfile.csv` file into the `Downloads` directory of the watsonx user. Once the data has been copied, you can use the MinIO interface to create a new Bucket and import the data into the bucket. At that point you can use the watsonx.data UI to catalog the data. 

See the section on [Minio usage](wxd-minio.md#using-the-minio-console-ui) for details of how MinIO works. An example of creating a new bucket can be found in the [Object Storage](wxd-objectstore.md#create-new-bucket-in-minio) section. 

Once you have loaded your data into the storage bucket, you will need to catalog this data into the watsonx.data system.

## Great Outdoors Company

The Sample Outdoors Company, or GO Sales, or any variation of the Sample Outdoors name, is the name of a fictitious business operation whose sample data is used to develop sample applications for IBM® and IBM customers. Its fictitious records include sample data for sales transactions, product distribution, finance, and human resources. Any resemblance to actual names, addresses, contact numbers, or transaction values, is coincidental. 

Two links that provide more details on the database.

* <a href="https://www.ibm.com/docs/en/cognos-analytics/11.1.0?topic=packages-sample-outdoors-company" target="_blank">Great Outdoors Company</a>
* <a href="https://www.ibm.com/docs/en/data-studio/4.1.1?topic=database-gsdb-reference" target="_blank">Great Outdoors Database Reference</a>

The second link will say that there is no content available, but if you click on the down arrow you will see the table names.

## Airline Report Carrier On-Time Performance Dataset

The Airline On-Time performance database contains information on flights within the US from 1987 through 2020. This is a very large dataset, so only the records from January 2013 has been included inside this image.

The following link provides more information on the dataset and the columns that are found in the records. Note that in the version of the data used in this system does not contain the diversion records 1 through 5. These fields are blank in the data sample used. Note that the initial diversion airport does exist in the record.

<a href="https://dax-cdn.cdn.appdomain.cloud/dax-airline/1.0.1/data-preview/index.html" target="_blank">Airline Report On-Time Performance Dataset</a>


#### Aircraft

|Column|Type
|------|------|
|TAIL_NUMBER| varchar| 
|MANUFACTURER| varchar| 
|MODEL| varchar

#### Airline ID

|Column|Type
|------|------|
|Code| int| 
|Description| varchar|

#### Airport ID

|Column|Type
|------|------|
|Code| int| 
|Description| varchar|

#### Cancellation
|Column|Type
|------|------|
|Code| int| 
|Description| varchar|

### Ontime
|Column|Type
|------|------|
|Year| int| 
|Quarter| int| 
|Month| int| 
|DayofMonth| int| 
|DayOfWeek| int| 
|FlightDate| varchar| 
|Reporting_Airline| varchar| 
|DOT_ID_Reporting_Airline| int| 
|IATA_CODE_Reporting_Airline| varchar| 
|Tail_Number| varchar| 
|Flight_Number_Reporting_Airline| int| 
|OriginAirportID| int| 
|OriginAirportSeqID| int| 
|OriginCityMarketID| int| 
|Origin| varchar| 
|OriginCityName| varchar| 
|OriginState| varchar| 
|OriginStateFips| varchar| 
|OriginStateName| varchar| 
|OriginWac| int| 
|DestAirportID| int| 
|DestAirportSeqID| int| 
|DestCityMarketID| int| 
|Dest| varchar| 
|DestCityName| varchar| 
|DestState| varchar| 
|DestStateFips| varchar| 
|DestStateName| varchar| 
|DestWac| int| 
|CRSDepTime| int| 
|DepTime| int| 
|DepDelay| int| 
|DepDelayMinutes| int| 
|DepDel15| int| 
|DepartureDelayGroups| int| 
|DepTimeBlk| varchar| 
|TaxiOut| int| 
|WheelsOff| int| 
|WheelsOn| int| 
|TaxiIn| int| 
|CRSArrTime| int| 
|ArrTime| int| 
|ArrDelay| int| 
|ArrDelayMinutes| int| 
|ArrDel15| int| 
|ArrivalDelayGroups| int| 
|ArrTimeBlk| varchar| 
|Cancelled| int| 
|CancellationCode| int| 
|Diverted| int| 
|CRSElapsedTime| int| 
|ActualElapsedTime| int| 
|AirTime| smallint| 
|Flights| int| 
|Distance| int| 
|DistanceGroup| int| 
|CarrierDelay| int| 
|WeatherDelay| int| 
|NASDelay| int| 
|SecurityDelay| int| 
|LateAircraftDelay| int| 
|FirstDepTime| int| 
|TotalAddGTime| int| 
|LongestAddGTime| int| 
|DivAirportLandings| int| 
|DivReachedDest| int| 
|DivActualElapsedTime| int| 
|DivArrDelay| int| 
|DivDistance| int| 
|DivAirport| varchar|

## Chicago Taxi Data

Taxi trips are reported to the City of Chicago in its role as a regulatory agency. To protect privacy but allow for aggregate analyses, the Taxi ID is consistent for any given taxi medallion number but does not show the number.

The data set used in this system contains records from January 1st, 2013 and does not include the census tract value nor the Taxi ID.

<a href="https://data.cityofchicago.org/Transportation/Taxi-Trips/wrvz-psew" target="taxis">Taxi Trips</a>

#### Taxi Data

|Column|Type
|------|------|
|TRIP_ID| int|
|COMPANY| varchar|
|DROPOFF_LATITUDE| double|
|DROPOFF_LONGITUDE| double|
|EXTRAS| double|
|FARE|   double|
|PAYMENT_TYPE| varchar|
|PICKUP_LATITUDE| double|
|PICKUP_LONGITUDE| double|
|TIPS|  double|
|TOLLS| double|
|TRIP_END_TIMESTAMP| timestamp|
|TRIP_MILES| double|
|TRIP_SECONDS| int|
|TRIP_START_TIMESTAMP| timestamp|
|TRIP_TOTAL| double

## Disclaimers

#### Great Outdoors

The Sample Outdoors Company, or GO Sales, or any variation of the Sample Outdoors name, is the name of a fictitious business operation whose sample data is used to develop sample applications for IBM® and IBM customers. Its fictitious records include sample data for sales transactions, product distribution, finance, and human resources. Any resemblance to actual names, addresses, contact numbers, or transaction values, is coincidental. Unauthorized duplication is prohibited.

#### Ontime Airline Database

Except as expressly set forth in this agreement, the data (including enhanced data) is provided on an “as is” basis, without warranties or conditions of any kind, either express or implied including, without limitation, any warranties or conditions of title, non-infringement, merchantability or fitness for a particular purpose.

Neither you nor any data providers shall have any liability for any direct, indirect, incidental, special, exemplary, or consequential damages (including without limitation lost profits), however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use or distribution of the data or the exercise of any rights granted hereunder, even if advised of the possibility of such damages.

#### Taxi Data

This site provides applications using data that has been modified for use from its original source, www.cityofchicago.org, the official website of the City of Chicago.  The City of Chicago makes no claims as to the content, accuracy, timeliness, or completeness of the data provided at this site.  The data provided at this site is subject to change at any time.  It is understood that the data provided at this site is being used at one’s own risk.








