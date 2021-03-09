---
title: Data formats
---

## File Types

The main storage format for survey data should be JSON. This format allows for storage of metadata directly alongside the survey data itself, ensuring that this information is not lost, and is easily read by machines.

As JSON is less "human readable", in certain circumstances it may be beneficial to provide the data in Excel or CSV formats. If this is the case, this information should only ever be provided **in addition to** the JSON files, with clear information provided to the recipient that the JSON files are the primary format.

## Basic Structure and Mandatory Fields

The exact structure will vary according to the type of survey undertaken, however some fields are required across all survey types.


### Mandatory Metadata Fields

| Field        | Description                                                                   | Example            |
|--------------|-------------------------------------------------------------------------------|--------------------|
| survey_type  | The type of survey data contained in this file. _Canonical list coming soon_. | ATC         |
| start_date   | The start date of the survey covered by this file, in `YYYY-MM-DD` format.    | 2019-11-01         |
| end_date     | The end date of the survey covered by this file, in `YYYY-MM-DD` format.      | 2019-11-08         |
| collected_by | The survey company responsible for data collection.                           | Tyrell Corporation |

#### Incidents

For the benefit of future users, any incidents occurring at or near the survey location which may have a material affect on the results must be included as part of the metadata for any and all relevant files.

#### Failures / Gaps

Any instances of failures in automated data collection or gaps in surveys must be included as part of the metadata for any and all relevant files.


#### Example
```json
{
    "metadata": {
        "survey_type": "ATC",
        "collected_by": "Tyrell Corporation",
        "start_date": "2019-11-01",
        "end_date": "2019-11-08"
    },
    "data": [
        {
            "period_start": "2019-11-01 07:00",
            "period_end": "2019-11-01 07:15",
            "vehicle_type": "Car",
            "count": 30
        },
        {
            "period_start": "2019-11-01 07:15",
            "period_end": "2019-11-01 07:30",
            "vehicle_type": "Car",
            "count": 34
        }
    ]
}
```