<?php
session_start();
include("header.php");
include("entrar.php");
if ($_SERVER["REQUEST_METHOD"] == "POST") {

    // 2. Coletar os dados enviados pelo formulário
    // Usamos htmlspecialchars para prevenir ataques XSS (Cross-Site Scripting)
    $email = htmlspecialchars(trim($_POST['email']));
    $password = htmlspecialchars(trim($_POST['password']));

    // 3. Validação básica (no lado do servidor)
    if (empty($email) || empty($password)) {
        $_SESSION['login_message'] = "Por favor, preencha todos os campos.";
        header("Location: login.html"); // Redireciona de volta para a página de login
        exit(); // Garante que o script pare de executar
    }

    // 4. Conexão com o Banco de Dados (Exemplo com MySQLi - para um sistema real)
    // Substitua com as suas credenciais e nome do banco de dados
    $servername = "localhost";
    $username = "seu_usuario_bd"; // Seu usuário do banco de dados
    $db_password = "sua_senha_bd";   // Sua senha do banco de dados
    $dbname = "seu_banco_de_dados"; // O nome do seu banco de dados

    // Criar conexão
    $conn = new mysqli($servername, $username, $db_password, $dbname);

    // Verificar conexão
    if ($conn->connect_error) {
        die("Conexão falhou: " . $conn->connect_error);
    }

    // 5. Preparar e executar a consulta SQL (usando Prepared Statements para segurança contra SQL Injection)
    $stmt = $conn->prepare("SELECT id, email, senha FROM usuarios WHERE email = ?");
    $stmt->bind_param("s", $email); // "s" indica que é uma string
    $stmt->execute();
    $result = $stmt->get_result();

    if ($result->num_rows == 1) {
        // Usuário encontrado, verificar a senha
        $user = $result->fetch_assoc();
        // password_verify é a função correta para verificar senhas hash em PHP
        if (password_verify($password, $user['senha'])) {
            // Login bem-sucedido!
            $_SESSION['user_id'] = $user['id'];
            $_SESSION['user_email'] = $user['email'];
            $_SESSION['login_message'] = "Login realizado com sucesso!";
            header("Location: dashboard.php"); // Redireciona para uma página pós-login
            exit();
        } else {
            // Senha incorreta
            $_SESSION['login_message'] = "Credenciais inválidas.";
            header("Location: login.html"); // Redireciona de volta para a página de login
            exit();
        }
    } else {
        // Usuário não encontrado
        $_SESSION['login_message'] = "Credenciais inválidas.";
        header("Location: login.html"); // Redireciona de volta para a página de login
        exit();
    }

    // Fechar a conexão e o statement
    $stmt->close();
    $conn->close();

} else {
    // Se o formulário não foi enviado via POST, redireciona de volta para o login
    header("Location: login.html");
    exit();
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head> 
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="projeto.css">
    <title>Espectro</title>
    <link rel="stylesheet" href="npm i bootstrap/@5.3.6">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.6/dist/css/bootstrap.min.css">
</head>
 <body>
     <div class="login-container">
        <h2>Entrar</h2>
        <form class="login-form">
            <div class="input-group">
                <label for="email">E-mail ou Usuário</label>
                <input type="text" id="email" name="email" required>
            </div>
            <div class="input-group">
                <label for="password">Senha</label>
                <input type="password" id="password" name="password" required>
            </div>
            <button type="submit" class="login-button">Entrar</button>
            <div class="links">
                <a href="#">Esqueceu a senha?</a>
                <span>Não tem uma conta? <a href="#">Cadastre-se</a></span>
            </div>
        </form>
    </div>
 </body>
</html>
