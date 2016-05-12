# ctaData
The purpose of this project is to document geospatial fix data collected from the Chicago Transit Authority via their public API.

## Background
Since 2009, the Chicago Transit Authority (CTA) has provided a [public API](http://www.transitchicago.com/developers/bustracker.aspx) that provides geospatial fixes for all of their buses. These fixes are updated every minute. The intent of this API is to give developers the ability to create "tracker apps" for mobile devices that provided estimated arrival time, bus route information, warnings for delays, etc.

The [ctaTracker project](https://github.com/jolson7168/ctaTracker) is a Python application that has been running in a continuous loop requesting this information every minute since Feburary 2015. Every night at midnight, it archives all the daya collected the previous day, and uploads it to a publicly accessible bucket on Amazon's S3. 

## Data Format
The daily archive files can be accessed in S3:

```
https://s3.amazonaws.com/cta-tracker/<YYYYMMDD>.zip
```

where YYYYMMDD is the format of the day you are requesting. 

The data set starts at 20150212 and runs until today. 
Each weekday is approximately 40MB compressed, and each weekend day is about 30MB compressed.

An average weekday will consist of approximately 800,000 fixes, representing a fleet size of approximately 1300 vehicles at peak rush hour times.

The JSON format of a daily archive file:

```
{
    "fixes":[
            {"vid": 1958, "tmstmp": "20150211 23:59", "lat": 41.880638122558594, "lon": -87.738800048828125, "hdg": 267, "pid": 949, "rt": "20", "des": "Austin", "pdist": 3429, "tablockid": "N20 -893", "tatripid": 1040830, "zone": "null”},
            .
            .
            .
    ]
}
```

The CTA documentation for the fields can be found in [this document](http://www.transitchicago.com/assets/1/developer_center/BusTime_Developer_API_Guide.pdf)

## Issues

On or about Dec 19, 2015, the CTA changed the format of their request. The collection code was not updated to reflect this change until Jan 16, 2016. During that period, there is less than the normal amount of data.

On Apr 28, 2016, the data collection machine was accidently deleted, translating into no data collected for Apr 29 2016 to May 10 2016. 

## Project Collaborators

[Smart Chicago Collaborative](http://www.smartchicagocollaborative.org/) - providing the S3 bucket.

[KPIT](https://www.kpit.com/) - providing the EC2 machine for collecting the data

