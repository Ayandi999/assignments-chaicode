# Healthcare System Database Documentation

This document provides a detailed overview of the database schema for the Healthcare Management System as defined in `code.eraseio`.

## Primary Entities

### customers
**Purpose**: Stores patient-specific information for identification, communication, and secure access.
- `customer_id`: Unique identifier (Serial PK).
- `customer_first_name`, `customer_last_name`: Legal name of the patient.
- `customer_phone_number`, `customer_email`: Contact channels.
- `customer_password`: Authenticated access credentials.
- `customer_profile_photo`: Reference to patient imagery.

### doctors
**Purpose**: Manages medical practitioner profiles and specialties.
- `doctor_nmr_id`: Unique National Medical Register ID (PK).
- `doctor_first_name`, `doctor_last_name`: Practitioner name.
- `doctor_contact_number`, `doctor_email`: Professional contact information.
- `doctor_speciality`: Area of medical expertise.
- `doctor_profile`: Reference to professional bio or imagery.

### services
**Purpose**: Connects doctors to specific service types and manages time-based availability.
- `service_id`: Unique identifier (Serial).
- `service_name`: Type of service provided (e.g., Consultation, Surgery).
- `doctor_id`: Reference to the providing doctor (FK).
- `start_time_slot`, `end_time_slot`: Defines the operational window.

### apponments
**Purpose**: The central entity linking patients, doctors, and scheduling logic.
- `appoinment_id`: Unique identifier (Serial PK).
- `customer_id`: Reference to the patient (FK).
- `doctor_id`: Reference to the doctor (FK).
- `date_of_appoinment`, `timeslot`: Temporal data for the booking.
- `payment_id`: Reference to the financial transaction (FK).

### payment
**Purpose**: Tracks financial transactions associated with medical appointments.
- `payment_id`: Internal unique identifier (PK Serial).
- `transaction_id`: External reference for payment processing (PK).
- `payment_method`: Channel used (e.g., Credit, Insurance).
- `appoinment_id`: Reference to the related booking (FK).

### consultation
**Purpose**: Records the clinical outcome of a completed appointment.
- `consultation_id`: Unique identifier (Serial PK).
- `appoinment_id`: Link to the source booking (FK).
- `prescription`: Medical advice and drug orders.
- `next_appoinment`: Follow-up scheduling.
- `ordered_test`: Link to diagnostic requirements (FK).

## Diagnostic and Lab Entities

### labs
**Purpose**: Catalog of registered laboratory facilities.
- `lab_regsitration_number`: Official registration ID (PK).
- `lab_name`, `lab_location`: Facility identification details.

### test_orders
**Purpose**: Root record for a diagnostic request containing multiple tests.
- `order_id`: Unique identifier (Serial).
- `doctor_id`: Requesting practitioner (FK).
- `customer_id`: Target patient (FK).
- `status`: Current state (e.g., Pending, Completed).

### orders
**Purpose**: Individual test line items within a diagnostic order.
- `order_id`: Parent order reference (FK).
- `test_id`: Unique test identifier.
- `test_name`: Description of the specific test (e.g., Blood Panel).
- `lab_id`: Assigned laboratory (FK).

### results
**Purpose**: The clinical data returned after lab processing.
- `order_id`, `test_id`: Composite reference to the specific test (FK).
- `test_result`: Qualitative or quantitative data.
- `is_reviewd`: Practitioner validation flag.

## Records and History

### paitient_records
**Purpose**: Archive of diagnostic outcomes and prescriptions for legal and clinical auditing.
- `record_id`: Archive entry identifier (Serial).
- `patient_id`, `doctor_id`: Entity references (FK).
- `diagonosis_provided`, `drugs_prescribed`, `tests_prescribed`: Comprehensive clinical summary.
- `test_order_id`: Reference to supporting diagnostics (FK).

### paitient_history
**Purpose**: Longitudinal tracking of patient vitals and background medical conditions.
- `data_id`: History entry identifier (Serial).
- `paitent_id`: Target patient (FK).
- `paitent_height`, `paitent_weight`: Basic vitals.
- `paitent_description`, `patients_past_history`: Qualitative background data.
- `recorded_by`: Reference to the practitioner (FK).
- `recorded_at`: Timestamp of data capture.

## Key Relationships

1. **Scheduling Engine**: `apponments` acts as a multi-way junction between `customers`, `doctors`, and `payment`.
2. **Clinical Workflow**: `consultation` follows `apponments`, optionally triggering `test_orders`.
3. **Diagnostic Chain**: `test_orders` -> `orders` -> `results`, with `orders` linked to specific `labs`.
4. **Data Continuity**: `paitient_records` and `paitient_history` consolidate data from across the system for a per-patient clinical view.
