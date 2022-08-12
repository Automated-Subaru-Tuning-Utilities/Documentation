# Documentation

## Introduction

The goal of this project is to automate some of the boring tasks associated with ECU tuning. You provide the log file, and we do the calculations/analysis. This is a modern alternative to the traditional excel spreadsheet that tuners commonly use. We are specifically targetting turbocharged Subarus compatible with ECUflash.

This is meant to be a tool suplimental to the tuning process. The goal is not to automate the tuning process, but rather to provide tuners with tools to speed up their workflows.  

## Functional Requirements

1. Establishing an Air-Fuel Relationship

## Requirement Modeling

1. Scaling the Bottom of the Maf-Curve
2. Injector Scaling
3. Scaling the Top of the Maf-Curve

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
            "Time (msec)":,
            "AF Correction #1 (%)":,
            "AF Correction Learning (%)":,
            "Intake Air Temperature (C)":,
            "Mass Airflow Sensor Voltage (V)":,
            "CL/OL Fueling* (status)":,
        }
    ]
}
```

#### UI/UX

### Backend
