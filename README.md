# **Documentation**

## **Introduction**

The goal of this project is to automate some of the tasks associated with ECU tuning. You provide the log file, and we do the calculations/analysis. This is a modern alternative to the traditional excel spreadsheet that tuners commonly use. We are specifically targetting turbocharged Subarus compatible with ECUflash and Romraider.

This is meant to be a tool suplimental to the tuning process. The goal is not to automate the tuning process, but rather to provide tuners with tools to speed up their workflows in a modern web application experience.   

## **Requirement Modeling**

1. Scaling the Bottom of the Maf-Curve
2. Tip in Enrichment Tuning 
3. Scaling the Top of the Maf-Curve
4. Tuning Boost - Visualizing data

## **Design**

### **System Design**

We will be using a microservice arcitecture to allow for scalability and choice of programming language/technology. Sepcifically, we will be using the API gateway design pattern in order to provide a single entry point for our microservices, providing load balancing capabilites and the ability to convert between protocols. The API gateway will also serve our frontend's static build files.  

![Microservice Architecture](Diagrams/SystemDesign.png?raw=true "Diagram")

We will be using Docker to containerize our application, and we will be looking into Kubernetes k8s for orchestration. 


### **Frontend - Component Level Design**

**Data Normaliztion Module**

The Data Normalization Module takes user input in the form of csv log files. It aims to accomplish 3 goals:
1. Normalize & clean the data
2. Error Check
3. Parse the data and prepare json api requests

![Data Normalization Module Component Level Diagram](Diagrams/DataNormalizationModule.drawio.png?raw=true "Diagram")

The Data Normalization Module will have standard api request structure for each type of use case. This is specified out in the backend documentation. 

**Boost Tuning Visualization**

TODO

**UI/UX**

TODO

### **Backend - Component Level Design**

**Lowmaf Microservice**

#### Context

Tuning the bottom of the maf curve is normally one of the first stages of tuning a vehicle. It helps establish the air-fuel relationship during closed loop driving

#### Request Structure
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
#### Response Structure
```json
[
  {
    "MafVoltage": float,
    "Correction": float,
    "Freqency": int
  },
  ...
]
```
**Tip in Enrichment Microservice**

#### Context
Tip in Enrichment is an ecu field which controls fuel during rapid changes in throttle.
"As the throttle angle changes suddenly there is an extra burst of air that enters the engine, so you need extra fuel to deal with the extra air. This table compensates for that" 

#### Steps for Tuning
1. Go out and log closed loop fueling 
2. Log: 
  * Throttle Position
  * Throttle angle change
  * af correction 1
  * af correction learned
3. Group the logs by throttle angle change. If for a particular throttle angle change the sum of the two corrections is > 14%, then take the correction, half it, and that is the resulting correction factor
ex. if the correction sum is 20%, the correction factor is .1. so multiply the current value in the table by .9

#### Request Structure
```json
[
{
"throttle_position": float,
"throttle_angle_change": float,
"af_correction_short": float,
"af_correction_learning": float
},
...
]
```

#### Response Structure
```json
[
{
"throttle_angle_change": float,
"correction": float,
"frequency": int
},
...
]
```

**Topmaf Microservice**

Scaling the top of the maf curve requires a wideband o2 sensor and some logs of WOT and top end pulls. 
We want data from ~2.66v -> top on the maf

* Request Body
```json
[

[
{
  "load": float,
  "rpm": int,
  "target_afr": float
},
...
],
{
  "maf_voltage": float,
  "throttle_position": float,
  "load": float,
  "rpm": int,
  "wideband_o2": float
},
...

]
```

* Response Body
```json 
[

{
  "MafVoltage": float,
  "Correction": float,
  "Frequency": int
},
...

]