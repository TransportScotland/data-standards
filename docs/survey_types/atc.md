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
- Vehicle classification descriptions

### Data
- Start time/date
- End time/data
- Vehicle class
- Count

## Basic Checks

### Preparing the data
Most checks should be undertaken through use of hourly totals, though it may also be beneficial to check data at shorter time intervals if collected.

### Tidality
Vehicle counts, particularly in locations near to populous areas, can be very "tidal" - large flows in one direction in the morning, and then in the other direction in the evening. Where this occurs, the direction of flows should be checked to confirm that the directions reported are correct.

### Vehicle type proportions
In most circumstances vehicle types relating to "cars" should provide the majority of flow at any given location. This is most prevalent during the daytime hours - at a minimum, any instance of "non-car" vehicle counts exceeding "car" vehicle counts for a one-hour period between 7am and 7pm (inclusive) should be investigated.

--8<-- "includes/abbreviations.md"