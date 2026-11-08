<?php
session_start();
require_once 'db.php';

// Authentication Check
if (!isset($_SESSION['resident_id'])) {
    header("Location: login.php");
    exit;
}

// Handle Log Out process
if (isset($_POST['logout'])) {
    if (isset($_SESSION['log_id'])) {
        // Record logout timestamp in audit logs
        $updateStmt = $pdo->prepare("UPDATE resident_logs SET logout_time = NOW() WHERE log_id = ?");
        $updateStmt->execute([$_SESSION['log_id']]);
    }
    
    // Clear session and redirect back to login
    session_unset();
    session_destroy();
    header("Location: login.php");
    exit;
}

// Fetch user profile details
$stmt = $pdo->prepare("SELECT * FROM residents WHERE id = ?");
$stmt->execute([$_SESSION['resident_id']]);
$user = $stmt->fetch();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Barangay Resident Dashboard</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f7f6; margin: 0; padding: 40px; }
        .dashboard-container { background: white; padding: 30px; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); max-width: 600px; margin: auto; }
        h1 { color: #333; margin-top: 0; }
        .info-box { background: #f9f9f9; padding: 15px; border-left: 4px solid #007bff; margin: 20px 0; }
        .logout-btn { background: #dc3545; color: white; border: none; padding: 10px 20px; font-size: 16px; border-radius: 4px; cursor: pointer; }
        .logout-btn:hover { background: #c82333; }
    </style>
</head>
<body>
    <div class="dashboard-container">
        <h1>Welcome, <?= htmlspecialchars($user['fullname']) ?>!</h1>
        <p>You are successfully logged into the Barangay Resident Portal dashboard.</p>
        
        <div class="info-box">
            <p><strong>Email:</strong> <?= htmlspecialchars($user['email']) ?></p>
            <p><strong>Contact Number:</strong> <?= htmlspecialchars($user['contact_number'] ?? 'Not provided') ?></p>
            <p><strong>Address:</strong> <?= htmlspecialchars($user['address'] ?? 'Not provided') ?></p>
        </div>

        <form method="POST">
            <button type="submit" name="logout" class="logout-btn">Log Out</button>
        </form>
    </div>
</body>
</html>
