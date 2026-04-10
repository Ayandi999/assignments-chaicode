# Elevator System Database Design

This document outlines the database schema for a multi-building elevator management system.

## Schema Overview

### 1. buildings
Stores the top-level information for each building complex in the system.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the building |
| name | varchar(50) | Name of the building |
| cluster_number | number | Identifies the physical location or complex |
| city | varchar(20) | City where the building is located |
| total_floors | number | Total number of floors in the structure |
| created_at | datetime | Timestamp when the record was created |

### 2. floors
Defines the specific floors associated with each building.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the floor |
| building_id | number (FK) | Reference to the building |
| floor_number | number | The numerical level of the floor |

### 3. elevator_shafts
Represents the vertical shafts within a building that house elevators.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the shaft |
| building_id | number (FK) | Reference to the building |
| shaft_code | number | Internal code or number for the shaft |

### 4. elevators
Contains details for each individual elevator unit.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the elevator |
| shaft_id | number (FK) | Reference to the vertical shaft it operates in |
| capacity | number | Maximum weight or passenger capacity |
| installed_at | datetime | Date of installation |

### 5. elevator_floor_service
A mapping table that defines which elevators can access which floors.
| Column | Type | Description |
| :--- | :--- | :--- |
| service_id | number (PK) | Unique identifier for the service mapping |
| elevator_id | number (FK) | Reference to the elevator |
| floor_id | number (FK) | Reference to the floor it serves |

### 6. elevator_status
Tracks the real-time operational state of each elevator.
| Column | Type | Description |
| :--- | :--- | :--- |
| status_log | number (PK) | Unique identifier for the status record |
| elevator_id | number (FK) | Reference to the elevator |
| status | enum | Current state (working, under maintenance) |
| current_floor | number | Current position of the elevator |
| direction | varchar(50) | Current movement direction |
| updated_at | datetime | Last time the status was refreshed |

### 7. floor_requests
Logs the requests made by users from floor control panels.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the request |
| floor_id | number (FK) | Floor where the request originated |
| requested_floor | number | Destination floor requested |
| direction | varchar(50) | Up or down direction |
| status | enum | Current state of the request |
| created_at | datetime | Time the button was pressed |

### 8. ride_allocations
Records the system's decision on which elevator is assigned to a specific request.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the allocation |
| request_id | number (FK) | Reference to the original floor request |
| elevator_id | number (FK) | Reference to the assigned elevator |
| allocated_at | datetime | Time the assignment was made |

### 9. ride_logs
Stores historical data for completed rides for performance analytics.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | varchar(50) (PK) | Unique identifier for the ride log |
| allocation_id | number (FK) | Reference to the ride allocation |
| start_floor | number | Starting floor of the journey |
| end_floor | number | Ending floor of the journey |

### 10. maintenance_records
Logs all technical service and repair history for each elevator.
| Column | Type | Description |
| :--- | :--- | :--- |
| id | number (PK) | Unique identifier for the record |
| elevator_id | number (FK) | Reference to the elevator serviced |
| type | varchar(50) | Type of maintenance performed |
| description | text | Detailed technical notes |
| technician | varchar(100) | Name of the assigned technician |
| started_at | datetime | Start time of the work |
| completed_at | datetime | Completion time of the work |
