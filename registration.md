<?php
require_once 'db.php';
$error = '';
$success = '';

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $fullname = trim($_POST['fullname']);
    $email = trim($_POST['email']);
    $password = $_POST['password'];
    $contact = trim($_POST['contact_number']);
    $address = trim($_POST['address']);

    if (!empty($fullname) && !empty($email) && !empty($password)) {
        // Check if email already exists
        $checkStmt = $pdo->prepare("SELECT id FROM residents WHERE email = ?");
        $checkStmt->execute([$email]);
        
        if ($checkStmt->rowCount() > 0) {
            $error = "Email address is already registered.";
        } else {
            // Hash password securely
            $hashedPassword = password_hash($password, PASSWORD_DEFAULT);

            $insertStmt = $pdo->prepare("INSERT INTO residents (fullname, email, password, contact_number, address) VALUES (?, ?, ?, ?, ?)");
            if ($insertStmt->execute([$fullname, $email, $hashedPassword, $contact, $address])) {
                $success = "Registration successful! You can now <a href='login.php'>Login here</a>.";
            } else {
                $error = "Something went wrong. Please try again.";
            }
        }
    } else {
        $error = "Please fill in all required fields.";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Barangay Portal - Resident Registration</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f7f6; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }
        .card { background: white; padding: 30px; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); width: 100%; max-width: 450px; }
        h2 { text-align: center; color: #333; margin-bottom: 20px; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; color: #666; }
        .form-group input, .form-group textarea { width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box; }
        .btn { width: 100%; padding: 10px; background: #28a745; border: none; color: white; border-radius: 4px; font-size: 16px; cursor: pointer; }
        .btn:hover { background: #218838; }
        .error { color: red; font-size: 14px; text-align: center; margin-bottom: 10px; }
        .success { color: green; font-size: 14px; text-align: center; margin-bottom: 10px; }
        .link { text-align: center; margin-top: 15px; font-size: 14px; }
        .link a { color: #007bff; text-decoration: none; }
    </style>
</head>
<body>
    <div class="card">
        <h2>Resident Registration</h2>
        <?php if (!empty($error)): ?><div class="error"><?= htmlspecialchars($error) ?></div><?php endif; ?>
        <?php if (!empty($success)): ?><div class="success"><?= $success ?></div><?php endif; ?>
        
        <form action="register.php" method="POST">
            <div class="form-group">
                <label>Full Name *</label>
                <input type="text" name="fullname" required>
            </div>
            <div class="form-group">
                <label>Email Address *</label>
                <input type="email" name="email" required>
            </div>
            <div class="form-group">
                <label>Password *</label>
                <input type="password" name="password" required>
            </div>
            <div class="form-group">
                <label>Contact Number</label>
                <input type="text" name="contact_number">
            </div>
            <div class="form-group">
                <label>Address / Purok</label>
                <textarea name="address" rows="2"></textarea>
            </div>
            <button type="submit" class="btn">Register</button>
        </form>
        <div class="link">
            Already have an account? <a href="login.php">Log in here</a>
        </div>
    </div>
</body>
</html>
