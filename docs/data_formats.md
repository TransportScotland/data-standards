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
| incidents    | _See below_                                                                   | _See below_        |
| errors       | _See below_                                                                   | _See below_        |

#### Incidents

For the benefit of future users, any incidents occurring at or near the survey location which may have a material affect on the results must be included as part of the metadata for any and all relevant files.

Incidents may include (but are not limited to):

- diversions
- accidents
- roadworks


##### Required Fields for Incidents

Any incidents should be provided as a list of items, with each item containing the following fields:

| Field        | Description                                                                                                            | Example             |
|--------------|------------------------------------------------------------------------------------------------------------------------|---------------------|
| start        | The date and time at which the incident will have first started to affect survey results, in `YYYY-MM-DD HH:MM` format | 2019-11-01 07:55    |
| end          | The date and time from which the incident will no longer survey results, in `YYYY-MM-DD HH:MM` format                  | 2019-11-01 08:30    |
| description  | A description of the incident                                                                                          | Police road closure |

Should no incidents occur, **this key should still be included within the metadata, with an empty array as its corresponding value.**

#### Errors

Any instances of failures in automated data collection or gaps in surveys must be included as part of the metadata for any and all relevant files, with both broadly fitting into the category of "errors".

Errors could include:

- malfunctioning equipment
- obstructed or incomplete (e.g. for queue surveys) views for video footage

##### Required Fields for Errors

Any incidents should be provided as a list of items, with each item containing the following fields:

| Field        | Description                                                                                                         | Example                  |
|--------------|---------------------------------------------------------------------------------------------------------------------|--------------------------|
| start        | The date and time at which the error will have first started to affect survey results, in `YYYY-MM-DD HH:MM` format | 2019-11-01 07:55         |
| end          | The date and time from which the error will no longer survey results, in `YYYY-MM-DD HH:MM` format                  | 2019-11-01 08:30         |
| description  | A description of the incident                                                                                       | Camera obstructed by owl |

As with incidents, should no errors occur, **this key should still be included within the metadata, with an empty array as its corresponding value.**

#### Categorising Incidents and Errors

The definitions of "incidents" and "errors" are broadly similar. As a rule of thumb, something should be classified as an incident if the actual _collection_ of data has not been impacted but the data itself may have been. An error, on the other hand, broadly refers to anything that has prevented collection of data.


#### Example

=== "No incidents / errors"
    ```json
    {
        "metadata": {
            "survey_type": "ANPR",
            "collected_by": "Tyrell Corporation",
            "start_date": "2019-11-01",
            "end_date": "2019-11-08",
            "incidents": [],
            "errors": []
        }
    }
    ```


=== "With incidents / errors"
    ```json
    {
        "metadata": {
            "survey_type": "ANPR",
            "collected_by": "Tyrell Corporation",
            "start_date": "2019-11-01",
            "end_date": "2019-11-08",
            "incidents": [
                {
                    "start": "2019-11-01 07:55",
                    "end": "2019-11-01 08:30",
                    "description": "Police road closure"
                }
            ],
            "errors": [
                {
                    "start": "2019-11-01 21:15",
                    "end": "2019-11-01 23:45",
                    "description": "Camera obstructed by owl"
                },
                {
                    "start": "2019-11-02 06:00",
                    "end": "2019-11-02 06:50",
                    "description": "View obscured by excessive rain"
                }
            ],
        }
    }
    ```