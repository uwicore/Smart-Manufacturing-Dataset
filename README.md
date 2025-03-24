# Smart-Manufacturing-Dataset
The dataset available in this repository contains information about the messages generated and data exchanged between devices, robots, machinery, sensors, actuators, operators, controllers, etc. in a real industrial scenario through an industrial (wired and/or wireless) network. The dataset has been generated using the virtual model of a realistic industrial scenario representing a steel sheet pressing plant for manufacturing automobile doors. The virtual model has been developed using the Virtual Component software [1]. The dataset contains the data generated during a 19 hours of operation.

# Industrial Scenario
The digital model corresponds to a steel sheet pressing plant for manufacturing automobile doors. This scenario is based on a layout available in the catalogue of Visual Components software which models a real production line composed of three consecutive presses that transform steel sheets for automobile door production. This layout has been extended to model a complete production plant. In particular, the number of production lines has increased to three, and a warehouse where raw materials are stored before production and a shipping warehouse where the manufactured material is stored have been implemented. Figure 1.a shows a view of the complete industrial production plant. 


![image](https://github.com/user-attachments/assets/1b1197c0-464f-4f48-aadb-6412998af28b)


a) Plant view


![image](https://github.com/user-attachments/assets/091f4169-2061-4164-be1b-69c056d23e1f)


 b) AGV transporting material. 


![image](https://github.com/user-attachments/assets/6b70e338-1d03-466c-b4bb-3c091f11523e)


c) Press line and quality control.
Figure 1. Industrial production plant.

The warehouse uses an automatic stock management system. This system monitors the availability of stored material through sensors installed on the shelves. These sensors send a signal when a shelf is occupied or empty. The material is loaded and unloaded on the shelves using a crane. Three AGVs transport materials (steel sheets) from the receiving warehouse to the production lines. The AGVs are managed by an AGV manager. At the beginning of each production line, there are designated areas for the delivery of the material transported by the AGVs. Sensors installed at these areas automatically send a material request message to the warehouse's stock management system when they detect a shortage of materials. Upon receiving a material request message, the automatic stock management system prepares the materials. Once the materials are ready, the AGV manager sends a message to assign the transport task to one of the AGVs. The AGVs periodically transmit a message with their current position to the AGV controller to manage the movement of the AGVs and avoid potential collisions.
Robotic arms installed at the beginning of the production line, and before and after each press are responsible for handling the steel sheets in the production lines. Specifically, the sensors installed at the delivery area of each production line send a message to two robotic arms installed at the beginning of each production line when material is available. The robotic arms then load the steel sheets into the first press. These two robotic arms work synchronously. To achieve this synchronization, the robots periodically exchange position information. When the first press finishes its task, it sends a message to a robotic arm located after the press to transfer the sheets to the subsequent press within the production line. When the second press finishes its task, the sheets are transferred to the third press by another robotic arm in a similar way. At the end of each production line, a quality control inspection is performed on the manufactured piece using high-resolution cameras. If a defective piece is detected, a message is sent to a robotic arm which removes the piece. The remaining pieces are collected by operators and transported by a forklift to the shipping warehouse. The shipping warehouse also implements an automatic stock management system. As presented, the machinery and robots that participate in the production workflow interact and coordinate their work through the exchange of messages and signals to request materials or to indicate that a work starts or finishes, or that the piece being manufactured is ready for the next production step (for example, to be transported or to be moved to the next press, etc.).
The plant also implements a Central Monitoring System that collects data about the status and operation of the production processes, machinery, robots, and warehouses. This system provides a complete perspective on the operation of the plant. To collect the information, the different management systems implemented in the plant (such as the warehouse management system and the AGVs manager) send periodic reports to the Central Monitoring System. 
# Dataset
The dataset contains three csv files with structured information about the data exchange in the industrial scenario, the position of the devices, robots, machinery, sensors, actuators, operators, controllers, etc. in the plant during the whole simulation, and the logging of different events that take place in the plant (changes in the state of the different devices, machineries or production processes) and that can be related with the generation of data. The information contained in each file is organized as follows:
## File: data_communications.csv 
This file contains information about the generated packets with data to be exchanged between different nodes (devices, robots, machinery, sensors, actuators, operators, controllers, etc.) in the industrial scenario. It includes information about the source and destination of the data, the size of the message, the time at which the message is generated, whether the message is generated periodically, and their latency and reliability requirements, among others. The size of the message, periodicity, and latency and reliability requirements have been established based on the traffic characteristics and requirements presented in 3GPP 22.104 [ref]. Next table provides a detailed description of the data contained in the data_communications.csv file.

Table I. data_communications.csv data.

### Communication Data

| Column Name        | Description                                         | Values |
|--------------------|-----------------------------------------------------|--------|
| Iteration         | Sequential number                                   | integer |
| Time Stamp       | Logs the time when the data is generated             | real |
| Transmitter      | Source identifier                                    | string |
| Receiver        | Destination identifier                               | string |
| Message Type    | Unique message ID that identifies the type of message | string |
| Communication Type | Indicates whether the message is periodic or aperiodic | `"a"` (if aperiodic) / `"p"` (if periodic) |
| Period          | Periodicity                                          | `"-1"` (if aperiodic) / real (ms) |
| Data Size       | Size of the data to transmit                         | integer (MB) |
| Latency         | Latency requirement                                  | integer (ms) |
| Reliability     | Percentage of messages that should be delivered successfully within the latency limit | float (%) |
| Confirmation    | Indicates whether confirmation is needed for the transmission | `"TRUE"` (if ACK needed) / `"FALSE"` (if ACK not needed) |


## File: data_position.csv 
This file logs the position of the nodes (devices, robots, machinery, sensors, actuators, operators, controllers, etc.) in the industrial plant during the whole simulation. For static nodes, position is only logged at the beginning of the simulation. For mobile nodes, the position is checked periodically every 0.5 seconds. If there is a change compared to the previously recorded position, the new position is logged.
Table II. data_position.csv data.

### Data Position

| Field Name     | Description                                                   | Values     |
|---------------|---------------------------------------------------------------|-----------|
| Iteration     | Sequential number                                             | integer   |
| Name          | Component name                                                | string    |
| Time         | Time at which the data is logged (since the simulation began)  | real      |
| Position_X   | Coordinates of the component in millimeters, relative to the center of the scenario | real (mm) |
| Position_Y   |  | real (mm) |
| Position_Z   |  | real (mm) |
| Orientation_X | Component angular orientation in degrees (0.0 indicates alignment with the global reference frame) | real |
| Orientation_Y |  | real |
| Orientation_Z |  | real |

## File: data_states.csv  
This file registers the initial and new states of each node during the simulation.

Table III. data_states.csv data.

### States Data

| Field Name     | Description                                                   | Values     |
|---------------|---------------------------------------------------------------|-----------|
| Iteration     | Sequential number                                             | integer   |
| Time Stamp    | Time at which the state change ocurs                          | real      |
| Component     | Component that experiences a state change                     | string    |
| State         | Current operational state                                     | string    |

## Messages and data generated in the industrial scenario
The data and characteristics of the messages generated are described in the next table:
Table IV. Data generated in the industrial plant.

### Industrial Plant Messages

| Message                           | Tx                               | Rx                                 | Size (bytes) | Latency (ms) | Reliability (%) | ACK  | Period (ms) |
|-----------------------------------|----------------------------------|------------------------------------|-------------|-------------|----------------|------|-------------|
| Shelf sensor updates              | Shelf sensors                   | Crane Controller                   | 64          | 100         | 99.9999        | yes  | -           |
| Crane Commands                    | Crane Controller                | Crane                              | 1000        | 50          | 99.9999        | yes  | -           |
| Crane Status                      | Crane                           | Crane Controller                   | 64          | 1000        | 99             | yes  | 5000        |
| Production Line Material Request  | Feed Line Robot                 | Storage Controller                 | 1000        | 50          | 99.9999        | yes  | -           |
| AGVs Management                   | AGV Controller                  | AGV Units                          | 1000        | 50          | 99.9999        | yes  | -           |
| AGVs Status                       | AGV Units                       | AGV Controller                     | 250         | period      | 99.9999        | -    | 100         |
| AGVs Commands                     | AGV Units                       | AGV Controller                     | 1000        | 50          | 99.9999        | yes  | -           |
| Shelves Statistics                | Crane Controller                | Global Monitoring System           | 1000000     | period      | 99.9999        | yes  | 60000       |
| AGVs Statistics                   | AGV Controller                  | Global Monitoring System           | 1000000     | period      | 99.9999        | yes  | 60000       |
| Conveyor Sensor Update            | Input Line Conveyors            | Input Line Robots Controllers      | 40          | 50          | 99.9999        | yes  | -           |
| Feed Command                      | Input Line Robots Controllers   | Input Line Robots Arms             | 1000        | 50          | 99.9999        | -    | -           |
| Robot State                       | Input Line Robots Controllers   | Input Line Robots Controllers      | 1000        | 50          | 99.9999        | -    | -           |
| Conveyor Sensor Update            | Input Line Conveyors            | Feed Press Robot Controllers       | 40          | 50          | 99.9999        | yes  | -           |
| Press Status                      | Presses                         | Feed Press Robot Controllers       | 1000        | 50          | 99.9999        | yes  | -           |
| Conveyor Sensor Update            | Output Line Conveyors           | Feed Press Robot Controllers       | 40          | 50          | 99.9999        | yes  | -           |
| Quality Sensor Updates            | Quality Sensor                  | Camera Controller                  | 40          | 50          | 99.9999        | yes  | -           |
| Quality Camera Commands           | Camera Controller               | Camera                             | 1000        | 50          | 99.9999        | yes  | -           |
| Quality Camera Data               | Camera                          | Camera Controller                  | 7600000     | 66.7        | 99.99          | yes  | -           |
| Quality Result                    | Camera Controller               | Quality Robot Controller           | 1000        | 50          | 99.9999        | yes  | -           |
| Robot Statistics                  | Robot Controllers               | Global Monitoring System           | 100000      | 500         | 99.99          | yes  | 60000       |
| Press Statistics                  | Presses                         | Global Monitoring System           | 100000      | 500         | 99.99          | yes  | 60000       |
| Quality Statistics                | Camera Controller               | Global Monitoring System           | 100000      | 500         | 99.99          | yes  | 60000       |

# Contact
Feel free to contact the corresponding authors M.Carmen Lucas-Estañ (m.lucas@umh.es) or Javier Gozálvez (j.gozalvez@umh.es) if you have any question about the code.

# License
The dataset is protected under the CC-SA 4.0 license.
Copyright (c) 2025 Universidad Miguel Hernández de Elche
The datasets used in the research presented in this article, and distributed as CC-SA 4.0, have been created using the 3D simulation solution Visual Components Premium 4.0 (Release 4.10). The simulations for generating the datasets have been done using the simulation models available in Visual Components´ electronic catalog, which simulate real equipment. Use of Visual Components 4.0 (Release 4.10) requires a valid license and acceptance of the EULA.
VISUAL COMPONENTS OY SHALL IN NO EVENT BE LIABLE FOR THE USE OF THE DATASETS GENERATED WITH ITS SIMULATION SOLUTIONS AND SIMULATION MODELS AND THE RESULTS DERIVED THEREFROM.

# Acknowledgements
This work has been funded by European Union's Horizon Europe Research and Innovation programme under the ZeroSWARM project (No. 101057083), by MCIN/AEI/10.13039/ 501100011033 and the “European Union NextGenerationEU /PRTR” (TED2021-130436B-I00) and by Generalitat Valenciana (CIGE/2022/17).

# References

[1] Virtual Components website: https://www.visualcomponents.com/
