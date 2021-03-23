---
title: ATC
---

_Work in progress_

## General Guidelines
Data should be provided at intervals not exceeding one hour in length, with 15 minute intervals generally recommended for data to be used in microsimulation modelling.

## Data Structure
- One JSON file per counter

## Required Fields

### Metadata

In addition to the mandatory fields detailed in the [Data Formats](../data_formats.md) page, there are three additional metadata fields required for every ATC site, each of which is detailed in the following sections.

| Field           | Description                                                                                       | Example     |
|-----------------|---------------------------------------------------------------------------------------------------|-------------|
| location        | A location object, with specified keys.                                                           | _See below_ |
| directions      | An array containing the directions used in the count data, with specified keys.                   | _See below_ |
| classifications | An array containing the vehicle type classifications used in the count data, with specified keys. | _See below_ |

#### Location

The location object for each ATC site should be sufficiently detailed for it to be accurately located either through mapping software or from its description alone. This information should provided according to the _actual location used_ for the installation of the counter, in case this varies from the brief for any reason.

Coordinates should be provided in British National Grid (EPSG:27700) form.

| Field       | Description                                          | Example                                                |
|-------------|------------------------------------------------------|--------------------------------------------------------|
| description | Text-based description of the counter location.      | Port Dundas Road, 50m South of junction with Milton St |
| eastings    | x-coordinate of the counter location _as installed_. | 259016                                                 |
| northings   | y-coordinate of the counter location _as installed_. | 666122                                                 |

##### Example
```json
{
    "metadata": {
        "location": {
            "description": "Port Dundas Road, 50m South of junction with Milton St",
            "eastings": 259016,
            "northings": 666122
        }
    }
}
```

#### Directions

Each direction used in the data should be detailed within the metadata, with both the exact encoding used in the data _and_ a description of the direction to validate this.

| Field       | Description                             | Example               |
|-------------|-----------------------------------------|-----------------------|
| direction   | Direction as used in the data.          | NB                    |
| description | Description of the direction of travel. | Towards Milton Street |

##### Example
```json
{
    "metadata": {
        "directions": [
            {
                "direction": "NB",
                "description": "Towards Milton Street"
            },
            {
                "direction": "SB",
                "description": "Towards Cowcaddens Road"
            }
        ]
    }
}
```

#### Classifications

The majority of ATC surveys require categorised vehicle counts. To avoid ambiguity, the categorisation methods used by the counter should be detailed as clearly as possible within this key. 

| Field       | Description                        | Example |
|-------------|------------------------------------|---------|
| class       | Vehicle class as used in the data. | 1       |
| description | Description of the class.          | Car     |

##### Example
```json
{
    "metadata": {
        "classifications": [
            {
                "class": 1,
                "description": "Car"
            },
            {
                "class": 2,
                "description": "LGV"
            },
            {
                "class": 3,
                "description": "HGV"
            }
        ]
    }
}
```

### Data

Count data should be provided as an array with one entry per time period. Within this, the count should be specified for each direction and vehicle class.

| Field  | Description                                                                               | Example          |
|--------|-------------------------------------------------------------------------------------------|------------------|
| start  | The start of the time period being reported by this record, in `YYYY-MM-DD HH:MM` format. | 2019-11-01 07:00 |
| end    | The end of the time period being reported by this record, in `YYYY-MM-DD HH:MM` format.   | 2019-11-01 08:00 |
| counts | An array containing the observed counts, with specified keys.                             | _See below_      |

#### Counts

The reported counts include descriptions which should directly match up to the values as provided in the metadata.

It is also critical that **observed counts of zero are included, and "unobserved" periods are not recorded as zero**. The recipient of the data must be able to distinguish between gaps in the data (e.g. due to malfunctioning equipment) and actual periods with no observations.

| Field     | Description                                                                                 | Example |
|-----------|---------------------------------------------------------------------------------------------|---------|
| direction | Direction of travel. Must match up with directions specified in the [metadata](#directions) | NB      |
| class     | Vehicle class. Must match up with directions specified in the [metadata](#classifications)  | 1       |
| count     | Number of vehicles recorded under the given parameters.                                     | 15      |


## Basic Checks

### Preparing the data
Most checks should be undertaken through use of hourly totals, though it may also be beneficial to check data at shorter time intervals if collected.

### Tidality
Vehicle counts, particularly in locations near to populous areas, can be very "tidal" - large flows in one direction in the morning, and then in the other direction in the evening. Where this occurs, the direction of flows should be checked to confirm that the directions reported are correct.

### Vehicle type proportions
In most circumstances vehicle types relating to "cars" should provide the majority of flow at any given location. This is most prevalent during the daytime hours - at a minimum, any instance of "non-car" vehicle counts exceeding "car" vehicle counts for a one-hour period between 7am and 7pm (inclusive) should be investigated.

## Example Structure

```json
{
    "metadata": {
        "survey_type": "ATC",
        "start_date": "2019-11-01",
        "end_date": "2019-11-01",
        "collected_by": "Tyrell Corporation",
        "incidents": [],
        "errors": [],
        "location": {
            "description": "Port Dundas Road, 50m South of junction with Milton St",
            "eastings": 259016,
            "northings": 666122
        },
        "directions": [
            {
                "direction": "NB",
                "description": "Towards Milton Street"
            },
            {
                "direction": "SB",
                "description": "Towards Cowcaddens Road"
            }
        ],
        "classifications": [
            {
                "class": 1,
                "description": "Car"
            },
            {
                "class": 2,
                "description": "LGV"
            },
            {
                "class": 3,
                "description": "HGV"
            }
        ],
    },
    "data": [
        {
            "start": "2019-11-01 07:00",
            "end": "2019-11-01 08:00",
            "counts": [
                {
                    "direction": "NB",
                    "class": 1,
                    "count": 10
                },
                {
                    "direction": "NB",
                    "class": 2,
                    "count": 8
                },
                {
                    "direction": "NB",
                    "class": 3,
                    "count": 5
                },
                {
                    "direction": "SB",
                    "class": 1,
                    "count": 8
                },
                {
                    "direction": "SB",
                    "class": 2,
                    "count": 4
                },
                {
                    "direction": "SB",
                    "class": 3,
                    "count": 2
                }
            ]
        },
        {
            "start": "2019-11-01 08:00",
            "end": "2019-11-01 09:00",
            "counts": [
                {
                    "direction": "NB",
                    "class": 1,
                    "count": 10
                },
                {
                    "direction": "NB",
                    "class": 2,
                    "count": 8
                },
                {
                    "direction": "NB",
                    "class": 3,
                    "count": 5
                },
                {
                    "direction": "SB",
                    "class": 1,
                    "count": 8
                },
                {
                    "direction": "SB",
                    "class": 2,
                    "count": 4
                },
                {
                    "direction": "SB",
                    "class": 3,
                    "count": 2
                }
            ]
        }
    ]
}
```

--8<-- "includes/abbreviations.md"