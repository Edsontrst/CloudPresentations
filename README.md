# CloudPresentations
Pagina en donde se puede visualizar los distintas diapostivias presentadas en clase
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cloud Presentations - UNMSM</title>
    <style>
        :root {
            --primary: #5D4037; /* Marrón elegante */
            --secondary: #8D6E63;
            --background: #EFEBE9; /* Fondo claro */
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background-color: var(--primary);
            min-height: 100vh;
            position: relative;
        }

        .unmsm-header {
            background: white;
            padding: 1rem;
            text-align: center;
            border-bottom: 2px solid #000;
        }

        .unmsm-header img {
            height: 60px;
            margin-bottom: 0.5rem;
        }

        .facultad-text {
            color: #000000;
            font-size: 1.2rem;
            font-weight: 500;
            letter-spacing: 1px;
        }

        header {
            background: rgba(255, 255, 255, 0.9);
            padding: 1.5rem;
            box-shadow: 0 2px 15px rgba(0,0,0,0.1);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        .logo-icon {
            width: 50px;
            height: 50px;
        }

        .cloud-logo {
            fill: var(--primary);
        }

        .gallery {
            max-width: 1200px;
            margin: 3rem auto;
            padding: 0 1.5rem;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 2rem;
        }

        .card {
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.2s;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card-iframe {
            width: 100%;
            height: 250px;
            border: none;
            background: #f0f0f0;
        }

        .card-content {
            padding: 1.5rem;
        }

        .card-title {
            color: var(--primary);
            margin-bottom: 0.5rem;
            font-size: 1.1rem;
        }

        .upload-section {
            max-width: 600px;
            margin: 3rem auto;
            padding: 2rem;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: none;
        }

        .admin-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s;
        }

        .admin-btn:hover {
            background: #4E342E;
        }

        .password-container {
            position: relative;
            margin: 1rem 0;
        }

        .toggle-password {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            cursor: pointer;
            color: var(--primary);
        }

        form input {
            width: 100%;
            padding: 0.75rem;
            margin: 0.5rem 0;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
        }

        form button[type="submit"] {
            background: var(--secondary);
            color: white;
            border: none;
            padding: 1rem;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            font-size: 1.1rem;
            margin-top: 1rem;
        }
    </style>
</head>
<body>
    <div class="unmsm-header">
        <img src="https://upload.wikimedia.org/wikipedia/commons/9/9f/Logo_UNMSM.png" alt="Logo UNMSM">
        <div class="facultad-text">Facultad de Ciencias Matemáticas</div>
    </div>

    <header>
        <nav class="nav-container">
            <div class="logo">
                <svg class="logo-icon cloud-logo" viewBox="0 0 24 24">
                    <path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96z"/>
                </svg>
                <h1>Cloud Presentations</h1>
            </div>
            <button class="admin-btn" onclick="toggleUpload()">Modo Admin</button>
        </nav>
    </header>

    <div class="upload-section" id="uploadSection">
        <h2>Subir Nueva Presentación</h2>
        <form id="uploadForm" onsubmit="return handleUpload(event)">
            <div class="password-container">
                <input type="password" id="adminPass" placeholder="Contraseña de Administrador" required>
                <span class="toggle-password" onclick="togglePasswordVisibility()">👁️</span>
            </div>
            <input type="text" id="presentationTitle" placeholder="Título de la Presentación" required>
            <input type="text" id="embedCode" placeholder="URL de Google Slides (Modo Embed)" required>
            <button type="submit">Subir Presentación</button>
        </form>
    </div>

    <div class="gallery" id="gallery">
        <!-- Presentaciones se cargarán aquí -->
    </div>

    <script>
        const ADMIN_PASSWORD = "fundamentos de programacion";
        let isAdminMode = false;

        function toggleUpload() {
            const uploadSection = document.getElementById('uploadSection');
            isAdminMode = !isAdminMode;
            uploadSection.style.display = uploadSection.style.display === 'none' ? 'block' : 'none';
        }

        function togglePasswordVisibility() {
            const passwordField = document.getElementById('adminPass');
            passwordField.type = passwordField.type === 'password' ? 'text' : 'password';
        }

        function handleUpload(e) {
            e.preventDefault();
            
            const password = document.getElementById('adminPass').value.toLowerCase();
            if(password !== ADMIN_PASSWORD.toLowerCase()) {
                alert("Contraseña incorrecta");
                return false;
            }

            const title = document.getElementById('presentationTitle').value;
            let embedCode = document.getElementById('embedCode').value;

            // Convertir URL normal a embed
            if(embedCode.includes("/view")) {
                embedCode = embedCode.replace("/view", "/preview") + "?embed=true";
            }

            const newPresentation = document.createElement('div');
            newPresentation.className = 'card';
            newPresentation.innerHTML = `
                <iframe class="card-iframe" src="${embedCode}" frameborder="0" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
                <div class="card-content">
                    <h3 class="card-title">${title}</h3>
                    <p>Fecha: ${new Date().toLocaleDateString()}</p>
                </div>
            `;

            document.getElementById('gallery').prepend(newPresentation);
            document.getElementById('uploadForm').reset();
            toggleUpload();
            
            return false;
        }
    </script>
</body>
</html>
