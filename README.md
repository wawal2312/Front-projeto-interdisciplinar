<?php
session_start();
include("header.php");
include("entrar.php");
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

