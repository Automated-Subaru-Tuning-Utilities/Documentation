# Documentation

## Introduction

## Functional Requirements

## Requirement Modeling

## Design

### Software Architecture

### Frontend - Component Level Design

#### Data Normaliztion Module

The Data Normalization Module takes user input in the form of csv log files. It aims to accomplish 3 goals:
1. Normalize & clean the data
2. Error Check
3. Parse the data and prepare json api requests

![Data Normalization Module Component Level Diagram](Diagrams/DataNormalizationModule.drawio.png?raw=true "Diagram")

The Data Normalization Module will have standard api request structure for each type of use case
* LowMaf Scaling
```json
{
    [
        {
            "Time":,
            "Af Correction 1 (%)":,
            "Af Correction Learning (%)":,
            "Intake Air Temperature":,
            "MAF voltage":,
            "CL/OL status":,
            "Throttle Position:"
        }
    ]
}
```

#### UI/UX

### Backend
