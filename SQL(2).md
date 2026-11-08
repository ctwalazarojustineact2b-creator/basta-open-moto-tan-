CREATE DATABASE IF NOT EXISTS barangay_db;
USE barangay_db;

-- 1. Barangay Profile Info
CREATE TABLE IF NOT EXISTS barangay_info (
    id INT AUTO_INCREMENT PRIMARY KEY,
    barangay_name VARCHAR(100) NOT NULL,
    municipality VARCHAR(100) NOT NULL,
    province VARCHAR(100) NOT NULL,
    captain_name VARCHAR(100) NOT NULL,
    hall_address TEXT NOT NULL
);

INSERT INTO barangay_info (barangay_name, municipality, province, captain_name, hall_address) 
VALUES ('Barangay San Jose', 'Malolos', 'Bulacan', 'Hon. Roberto "Bobby" Santos', 'San Jose Barangay Hall, Malolos, Bulacan');

-- 2. Barangay Officials
CREATE TABLE IF NOT EXISTS officials (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50) NOT NULL,
    committee VARCHAR(100) NOT NULL,
    contact_number VARCHAR(20) NOT NULL
);

INSERT INTO officials (name, position, committee, contact_number) VALUES
('Hon. Roberto Santos', 'Punong Barangay', 'Executive / Overall Supervision', '0917-123-4567'),
('Maria Clara Reyes', 'Barangay Kagawad', 'Committee on Health & Sanitation', '0918-234-5678'),
('Juanito Dela Cruz', 'Barangay Kagawad', 'Committee on Peace and Order', '0919-345-6789'),
('Ana Marie Santos', 'Barangay Secretary', 'Administrative Records', '0920-456-7890');

-- 3. Residents Directory
CREATE TABLE IF NOT EXISTS residents (
    id INT AUTO_INCREMENT PRIMARY KEY,
    fullname VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    age INT NOT NULL,
    gender VARCHAR(20) NOT NULL,
    address TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO residents (fullname, email, password, age, gender, address) VALUES
('Carlos V. Gomez', 'carlos@example.com', '$2y$10$e0MYzXyjpJS7Pd0RVvHwHeFvGq1l8sFv6Vv7qS7bU3jW2L1m5K2y6', 34, 'Male', 'Purok 2, San Jose'),
('Elena P. Lim', 'elena@example.com', '$2y$10$e0MYzXyjpJS7Pd0RVvHwHeFvGq1l8sFv6Vv7qS7bU3jW2L1m5K2y6', 28, 'Female', 'Purok 4, San Jose');

-- 4. Blotter Records
CREATE TABLE IF NOT EXISTS blotter_records (
    id INT AUTO_INCREMENT PRIMARY KEY,
    complainant VARCHAR(100) NOT NULL,
    respondent VARCHAR(100) NOT NULL,
    incident_type VARCHAR(100) NOT NULL,
    details TEXT NOT NULL,
    status ENUM('Pending', 'Settled', 'Escalated') DEFAULT 'Pending',
    date_filed TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO blotter_records (complainant, respondent, incident_type, details, status) VALUES
('Pedro Penduko', 'Juan Tamad', 'Noise Disturbance', 'Loud karaoke past midnight.', 'Settled'),
('Maria Santos', 'Unknown', 'Property Damage', 'Fence graffiti reported.', 'Pending');

-- 5. Document Requests (Forms & Certificates)
CREATE TABLE IF NOT EXISTS document_requests (
    id INT AUTO_INCREMENT PRIMARY KEY,
    resident_name VARCHAR(100) NOT NULL,
    document_type ENUM('Barangay Clearance', 'Certificate of Indigency') NOT NULL,
    purpose VARCHAR(255) NOT NULL,
    status ENUM('Pending', 'Released') DEFAULT 'Pending',
    date_requested TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO document_requests (resident_name, document_type, purpose, status) VALUES
('Carlos V. Gomez', 'Barangay Clearance', 'Employment Requirements', 'Released'),
('Elena P. Lim', 'Certificate of Indigency', 'Medical Assistance', 'Pending');
