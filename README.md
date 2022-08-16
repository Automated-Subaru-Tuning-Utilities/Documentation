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

We will be using a microservice arcitecture to allow for scalability and choice of programming language/technology

![Microservice Architecture](Diagrams/microserviceArcitecture.drawio.png?raw=true "Diagram")

### Frontend - Component Level Design

#### Data Normaliztion Module

The Data Normalization Module takes user input in the form of csv log files. It aims to accomplish 3 goals:
1. Normalize & clean the data
2. Error Check
3. Parse the data and prepare json api requests

![Data Normalization Module Component Level Diagram](Diagrams/DataNormalizationModule.drawio.png?raw=true "Diagram")

The Data Normalization Module will have standard api request structure for each type of use case
* LowMaf Scaling Request Structure
```json
[
  {
    "time": int,
    "af_correction_short": float,
    "af_correction_learning": float,
    "intake_air_temperature": int,
    "mass_airflow_voltage": float,
    "cl_ol_fueling_status": int,
  },
  ...
]
```
* LowMaf Scaling Response Structure
```json
[
  {
    "MafVoltage": float,
    "Correction": float,
    "Freqency": int
  },
  ...
]



#### UI/UX

### Backend
