CREATE DATABASE IF NOT EXISTS barangay_db;
USE barangay_db;

-- Residents Table with additional typical barangay details
CREATE TABLE IF NOT EXISTS residents (
    id INT AUTO_INCREMENT PRIMARY KEY,
    fullname VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    contact_number VARCHAR(20) DEFAULT NULL,
    address TEXT DEFAULT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Login & Logout Activity Logs Table
CREATE TABLE IF NOT EXISTS resident_logs (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    resident_id INT NOT NULL,
    login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    logout_time TIMESTAMP NULL DEFAULT NULL,
    FOREIGN KEY (resident_id) REFERENCES residents(id) ON DELETE CASCADE
);
