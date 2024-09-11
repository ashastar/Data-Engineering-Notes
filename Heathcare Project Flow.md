# Healthcare Database Schema

## Database Name: `HealthcareManagementDB`

### Tables in the Database:

1. **Table Name:** `Patients`
   - **Description:** Stores patient personal information.
   - **Columns:**
     - `PatientID` (INT, Primary Key)
     - `FirstName` (VARCHAR)
     - `LastName` (VARCHAR)
     - `DOB` (DATE)
     - `Gender` (VARCHAR)
     - `ContactNumber` (VARCHAR)
     - `Email` (VARCHAR)
     - `Address` (VARCHAR)
     - `InsuranceNumber` (VARCHAR)

2. **Table Name:** `Doctors`
   - **Description:** Stores doctor information.
   - **Columns:**
     - `DoctorID` (INT, Primary Key)
     - `FirstName` (VARCHAR)
     - `LastName` (VARCHAR)
     - `Specialization` (VARCHAR)
     - `PhoneNumber` (VARCHAR)
     - `Email` (VARCHAR)
     - `Address` (VARCHAR)

3. **Table Name:** `Appointments`
   - **Description:** Stores information about patient appointments.
   - **Columns:**
     - `AppointmentID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `AppointmentDate` (DATETIME)
     - `Status` (VARCHAR) — e.g., Scheduled, Completed, Canceled
     - `Notes` (TEXT)

4. **Table Name:** `MedicalRecords`
   - **Description:** Stores patient medical history and records.
   - **Columns:**
     - `RecordID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `VisitDate` (DATETIME)
     - `Diagnosis` (VARCHAR)
     - `Treatment` (TEXT)
     - `Prescription` (TEXT)

5. **Table Name:** `Billing`
   - **Description:** Stores billing details for services provided to patients.
   - **Columns:**
     - `BillID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `AppointmentID` (INT, Foreign Key from Appointments)
     - `Amount` (DECIMAL)
     - `DateIssued` (DATETIME)
     - `Status` (VARCHAR) — e.g., Paid, Unpaid, Pending

6. **Table Name:** `Medications`
   - **Description:** Stores information about available medications.
   - **Columns:**
     - `MedicationID` (INT, Primary Key)
     - `Name` (VARCHAR)
     - `Dosage` (VARCHAR)
     - `Description` (TEXT)
     - `SideEffects` (TEXT)

7. **Table Name:** `Insurance`
   - **Description:** Stores details about insurance providers.
   - **Columns:**
     - `InsuranceID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `ProviderName` (VARCHAR)
     - `PolicyNumber` (VARCHAR)
     - `CoverageDetails` (TEXT)

8. **Table Name:** `LabTests`
   - **Description:** Stores information about lab tests prescribed to patients.
   - **Columns:**
     - `TestID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `TestName` (VARCHAR)
     - `TestDate` (DATETIME)
     - `Results` (TEXT)

### Relationships:
- **Patients ↔ Appointments**: A patient can have many appointments.
- **Doctors ↔ Appointments**: A doctor can have many appointments.
- **Patients ↔ MedicalRecords**: A patient can have many medical records.
- **Patients ↔ Billing**: A patient can have many bills.
- **Doctors ↔ MedicalRecords**: A doctor can be linked to many medical records.
- **Patients ↔ LabTests**: A patient can undergo many lab tests.
