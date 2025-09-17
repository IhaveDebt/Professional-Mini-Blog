<?php
$host = 'localhost';
$db   = 'blog';
$user = 'root';
$pass = '';
$charset = 'utf8mb4';
$dsn = "mysql:host=$host;dbname=$db;charset=$charset";

$options = [
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
];

try {
    $pdo = new PDO($dsn, $user, $pass, $options);
} catch (\PDOException $e) {
    exit('Database connection failed.');
}

// Insert new post
if ($_SERVER['REQUEST_METHOD'] === 'POST' && !empty($_POST['post'])) {
    $stmt = $pdo->prepare("INSERT INTO posts (content) VALUES (:content)");
    $stmt->execute(['content' => $_POST['post']]);
}

// Fetch posts
$stmt = $pdo->query("SELECT * FROM posts ORDER BY created_at DESC");
$posts = $stmt->fetchAll();
?>
<!DOCTYPE html>
<html>
<head>
    <title>Professional Blog</title>
    <style>body{font-family:Arial;margin:50px;} textarea{width:300px;height:80px;}</style>
</head>
<body>
    <h2>My Professional Blog</h2>
    <form method="post">
        <textarea name="post" placeholder="Write something..." required></textarea><br>
        <button type="submit">Post</button>
    </form>
    <h3>Posts:</h3>
    <ul>
        <?php foreach($posts as $p): ?>
            <li><?= htmlspecialchars($p['content']) ?> <small>(<?= $p['created_at'] ?>)</small></li>
        <?php endforeach; ?>
    </ul>
</body>
</html>
