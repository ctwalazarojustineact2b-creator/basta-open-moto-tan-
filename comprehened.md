<?php
session_start();
require_once 'db.php';

// Fetch Barangay Info
$stmtInfo = $pdo->query("SELECT * FROM barangay_info LIMIT 1");
$brgyInfo = $stmtInfo->fetch();

// Fetch Counts for Metrics
$totalResidents = $pdo->query("SELECT COUNT(*) FROM residents")->fetchColumn();
$totalBlotter = $pdo->query("SELECT COUNT(*) FROM blotter_records")->fetchColumn();
$totalRequests = $pdo->query("SELECT COUNT(*) FROM document_requests")->fetchColumn();
$releasedReports = $pdo->query("SELECT COUNT(*) FROM document_requests WHERE status = 'Released'")->fetchColumn();

// Fetch Data Arrays
$officials = $pdo->query("SELECT * FROM officials")->fetchAll();
$residents = $pdo->query("SELECT * FROM residents")->fetchAll();
$blotters = $pdo->query("SELECT * FROM blotter_records")->fetchAll();
$requests = $pdo->query("SELECT * FROM document_requests")->fetchAll();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title><= htmlspecialchars($brgyInfo['barangay_name']) ?> - Management Dashboard</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="bg-light">

    <!-- Top Navigation Bar -->
    <nav class="navbar navbar-dark bg-dark shadow-sm">
        <div class="container-fluid">
            <span class="navbar-brand mb-0 h1">🏛️ <?= htmlspecialchars($brgyInfo['barangay_name']) ?> Management Portal</span>
            <span class="text-white">Municipality of <?= htmlspecialchars($brgyInfo['municipality']) ?>, <?= htmlspecialchars($brgyInfo['province']) ?></span>
        </div>
    </nav>

    <div class="container my-4">
        
        <!-- Barangay Overview Info Card -->
        <div class="card mb-4 shadow-sm border-primary">
            <div class="card-header bg-primary text-white">
                <h5 class="mb-0">Barangay Profile & Captain Overview</h5>
            </div>
            <div class="card-body">
                <p><strong>Hall Address:</strong> <?= htmlspecialchars($brgyInfo['hall_address']) ?></p>
                <p><strong>Punong Barangay (Captain):</strong> <?= htmlspecialchars($brgyInfo['captain_name']) ?></p>
            </div>
        </div>

        <!-- Metric Cards -->
        <div class="row text-center mb-4">
            <div class="col-md-3">
                <div class="card bg-white shadow-sm p-3">
                    <h3><?= $totalResidents ?></h3>
                    <span class="text-muted">Registered Residents</span>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card bg-white shadow-sm p-3">
                    <h3><?= $totalBlotter ?></h3>
                    <span class="text-muted">Blotter Cases</span>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card bg-white shadow-sm p-3">
                    <h3><?= $totalRequests ?></h3>
                    <span class="text-muted">Total Form Requests</span>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card bg-white shadow-sm p-3">
                    <h3><?= $releasedReports ?></h3>
                    <span class="text-muted">Released Reports/Docs</span>
                </div>
            </div>
        </div>

        <!-- Section: Barangay Officials -->
        <div class="card mb-4 shadow-sm">
            <div class="card-header bg-secondary text-white">Barangay Officials</div>
            <div class="card-body">
                <table class="table table-striped">
                    <thead><tr><th>Name</th><th>Position</th><th>Committee</th><th>Contact Number</th></tr></thead>
                    <tbody>
                        <?php foreach($officials as $off): ?>
                        <tr>
                            <td><?= htmlspecialchars($off['name']) ?></td>
                            <td><?= htmlspecialchars($off['position']) ?></td>
                            <td><?= htmlspecialchars($off['committee']) ?></td>
                            <td><?= htmlspecialchars($off['contact_number']) ?></td>
                        </tr>
                        <?php endforeach; ?>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Section: Residents Directory -->
        <div class="card mb-4 shadow-sm">
            <div class="card-header bg-secondary text-white">Registered Residents Directory</div>
            <div class="card-body">
                <table class="table table-hover">
                    <thead><tr><th>Full Name</th><th>Age</th><th>Gender</th><th>Address</th><th>Email</th></tr></thead>
                    <tbody>
                        <?php foreach($residents as $res): ?>
                        <tr>
                            <td><?= htmlspecialchars($res['fullname']) ?></td>
                            <td><?= htmlspecialchars($res['age']) ?></td>
                            <td><?= htmlspecialchars($res['gender']) ?></td>
                            <td><?= htmlspecialchars($res['address']) ?></td>
                            <td><?= htmlspecialchars($res['email']) ?></td>
                        </tr>
                        <?php endforeach; ?>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Section: Blotter Records -->
        <div class="card mb-4 shadow-sm">
            <div class="card-header bg-danger text-white">Blotter Incident Records</div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead><tr><th>Complainant</th><th>Respondent</th><th>Incident Type</th><th>Details</th><th>Status</th></tr></thead>
                    <tbody>
                        <?php foreach($blotters as $b): ?>
                        <tr>
                            <td><?= htmlspecialchars($b['complainant']) ?></td>
                            <td><?= htmlspecialchars($b['respondent']) ?></td>
                            <td><?= htmlspecialchars($b['incident_type']) ?></td>
                            <td><?= htmlspecialchars($b['details']) ?></td>
                            <td><span class="badge bg-<?= $b['status'] == 'Settled' ? 'success' : 'warning' ?>"><?= $b['status'] ?></span></td>
                        </tr>
                        <?php endforeach; ?>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Section: Forms & Certificates (Barangay Clearance & Indigency) -->
        <div class="card mb-4 shadow-sm">
            <div class="card-header bg-success text-white">Forms & Certificate Requests</div>
            <div class="card-body">
                <p class="text-muted">Showing requested documents (e.g., Barangay Clearance, Certificate of Indigency):</p>
                <table class="table table-bordered">
                    <thead><tr><th>Resident Name</th><th>Document Type</th><th>Purpose</th><th>Status</th></tr></thead>
                    <tbody>
                        <?php foreach($requests as $req): ?>
                        <tr>
                            <td><?= htmlspecialchars($req['resident_name']) ?></td>
                            <td><strong><?= htmlspecialchars($req['document_type']) ?></strong></td>
                            <td><?= htmlspecialchars($req['purpose']) ?></td>
                            <td><span class="badge bg-<?= $req['status'] == 'Released' ? 'success' : 'secondary' ?>"><?= $req['status'] ?></span></td>
                        </tr>
                        <?php endforeach; ?>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Section: Released Reports -->
        <div class="card shadow-sm">
            <div class="card-header bg-dark text-white">Released Reports Audit</div>
            <div class="card-body">
                <p>Summary audit log tracking successfully issued clearance and indigency certificates.</p>
                <ul>
                    <?php foreach($requests as $r): if($r['status'] == 'Released'): ?>
                        <li>Issued <strong><?= htmlspecialchars($r['document_type']) ?></strong> to <em><?= htmlspecialchars($r['resident_name']) ?></em> for purpose: <?= htmlspecialchars($r['purpose']) ?></li>
                    <?php endif; endforeach; ?>
                </ul>
            </div>
        </div>

    </div>

</body>
</html>
