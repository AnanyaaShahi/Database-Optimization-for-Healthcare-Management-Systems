README - Healthcare Management System: A Johns Hopkins Case Study

**Project Overview**

The Healthcare Management System is designed to optimize patient care and pharmacy operations at Johns Hopkins Hospital. This system aims to streamline medical records, billing, insurance, prescriptions, and inventory management while ensuring regulatory compliance.

**Objective**

The primary objective of this project is to design and implement a comprehensive database management system (DBMS) to:

1. Manage patients, medical records, billing, insurance, pharmacy transactions, and inventory

2. Enhance data retrieval efficiency

3. Improve hospital workflows and patient satisfaction

4. Ensure regulatory compliance

5. Entities and Relationships

The database includes the following key entities:

* Patients (ID, Name, DOB, Contact Info)

* Medical Records (Allergies, Medications, Immunizations, History)

* Departments (ID, Name, Location, Budget, Contact)

* Employees (ID, Name, Specialization, Salary, Type)

* Physicians (ID, Name, Specialty, Salary)

* Nurses (ID, Name, Specialization, Salary, License Number)

* Billing & Insurance (Service Code, Date, Charge, Provider, Status)

* Prescriptions (Dosage, Date, Physician, Patient)

* Medical Supplies (ID, Name, Strength, Type, Manufacturer, Expiry Date)

* Inventory (Quantity, Price, Reorder Level, Last Reorder Date)

* Transactions (Prescription ID, Pharmacist, Date, Quantity Dispensed, Price)

* Pharmacists (ID, Name, License Number, Experience, Certifications)

**Business Rules**

* Each patient must have a unique medical record.

* Each physician must belong to at least one department.

* A patient can have multiple prescriptions.

* A prescription is associated with one physician and one or more medical supplies.

* Inventory must be updated when a prescription is dispensed.

* Transactions must be linked to a pharmacist and prescription.

  <img width="520" alt="Screenshot 2025-02-03 at 10 21 29 PM" src="https://github.com/user-attachments/assets/7ff95f6e-6973-4b59-81ae-a1d87600529e" />
<img width="703" alt="Screenshot 2025-02-03 at 10 21 14 PM" src="https://github.com/user-attachments/assets/cbbec084-5411-460e-940a-4bf9815d30a0" />


**Database Implementation**

**Schema Creation**

CREATE SCHEMA healthcare_db;
USE healthcare_db;

Sample Queries

**Query 1: Retrieve today's transactions**

SELECT TransactionID, TransactionDate, PrescriptionID, TotalPrice, PharmacistID  
FROM Transactions  
WHERE TransactionDate = CURRENT_DATE;

**Query 2: Average transaction amount per patient**

SELECT PatientID, PatientName,  
(SELECT AVG(ChargeAmount)  
 FROM BillingInsurance  
 WHERE PatientID = p.PatientID) AS AvgTransactionAmount  
FROM Patients p
ORDER BY AvgTransactionAmount DESC;

**Query 3: Top 3 highest-paid physicians and their prescription count**

SELECT p.PhysicianID, p.EmployeeName, COUNT(pr.PrescriptionID) AS NumberOfPrescriptions, p.Salary AS TotalSalary  
FROM Physician p  
LEFT JOIN Prescriptions pr ON p.PhysicianID = pr.PrescribingPhysicianID  
GROUP BY p.PhysicianID, p.EmployeeName  
ORDER BY p.Salary DESC  
LIMIT 3;

**Query 4: Retrieve physician names and specialties with department info**

SELECT p.EmployeeName AS PhysicianName, p.Specialization,  
(SELECT d.DepartmentName   
FROM Departments d   
WHERE d.DepartmentID = p.DepartmentID) AS DepartmentName  
FROM Physician p;

**Query 5: Find patients allergic to Aspirin or Penicillin**

SELECT COUNT(p.PatientID) AS PatientCount, p.PatientID, p.PatientName, m.Allergies  
FROM Patients p  
JOIN MedicalRecords m ON p.PatientID = m.PatientID  
WHERE m.Allergies LIKE '%Aspirin%' OR m.Allergies LIKE '%Penicillin%'  
GROUP BY p.PatientID, p.PatientName, m.Allergies;

**Query 6: Calculate inventory turnover rate per medication**

SELECT ms.MedicalSupplyID, ms.SupplyName, SUM(t.QuantityDispensed) AS TotalQuantityDispensed, AVG(i.QuantityOnHand) AS AverageQuantityAvailable,  
CASE  
    WHEN AVG(i.QuantityOnHand) > 0 THEN  
        SUM(t.QuantityDispensed) / AVG(i.QuantityOnHand)
    ELSE 0  
END AS InventoryTurnoverRate  
FROM Transactions t  
JOIN PrescriptionMedicalSupplies pms ON t.PrescriptionID = pms.PrescriptionID  
JOIN MedicalSupplies ms ON pms.MedicalSupplyID = ms.MedicalSupplyID  
JOIN Inventory i ON ms.MedicalSupplyID = i.MedicalSupplyID  
GROUP BY ms.MedicalSupplyID, ms.SupplyName;

**Challenges and Learnings**

**Challenges:**

1. Time Constraints: Limited time restricted deeper exploration of additional queries.

2. Domain Knowledge: Understanding the complex healthcare workflows was challenging.

3. Scope Complexity: Managing all hospital operations increased project difficulty.

**Learnings:**

1. Industry Insight: Learned about hospital operations from patient entry to inventory tracking.

2. Collaboration: Worked as a team to develop real-world database solutions.

3. Query Optimization: Improved SQL query writing and optimization skills.

