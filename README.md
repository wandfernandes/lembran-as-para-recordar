<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Caça ao Tesouro - Florianópolis</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background: url('https://drive.google.com/uc?export=view&id=1miMk1WTVpS_tOcfJNYrD-B_b2uxdAtc_') no-repeat center center fixed;
            background-size: cover;
            color: #fff;
            overflow-x: hidden;
        }
        h1 {
            font-family: 'Dancing Script', cursive;
            font-size: 3em;
            margin-top: 20px;
        }
        #inicio {
            margin-top: 50px;
            opacity: 1;
            transition: opacity 1s ease-in-out;
        }
        .fade-out {
            opacity: 0;
        }
        #pista-container {
            display: none;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
            padding: 20px;
            border-radius: 10px;
            margin: 20px auto;
            max-width: 600px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            animation: fadeIn 1s;
        }
        .avatar-selection img {
            cursor: pointer;
            width: 100px;
            border-radius: 50%;
            transition: transform 0.3s, border 0.3s;
        }
        .avatar-selected {
            border: 3px solid #ff4081;
            transform: scale(1.1);
        }
        .medalha {
            width: 80px;
            height: 80px;
            display: inline-block;
            margin: 10px;
            background-size: cover;
            opacity: 0;
            animation: fadeIn 1s forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div id="inicio">
        <h1>🌸 Caça ao Tesouro - Florianópolis 🌸</h1>
        <p>Escolha seu avatar romântico:</p>
        <div class="avatar-selection">
            <img src="https://drive.google.com/uc?export=view&id=1q1K0_alcBPs84HewUL54WS9WYf68lG_A" class="avatar" onclick="selecionarAvatar(this)">
            <img src="https://drive.google.com/uc?export=view&id=13gZHVmLwcQn4eq7rXo-19BQlboq6vZLV" class="avatar" onclick="selecionarAvatar(this)">
            <img src="https://drive.google.com/uc?export=view&id=1mh3j-23dMaKB2DtyyB5hE8ZUtRzfcHkk" class="avatar" onclick="selecionarAvatar(this)">
        </div>
        <p>Insira sua chave de acesso:</p>
        <input type="text" id="chave" placeholder="Digite sua chave">
        <button onclick="iniciarJogo()">Começar</button>
    </div>

    <div id="pista-container">
        <h2 id="pista"></h2>
        <p id="mensagem"></p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
    </div>

    <div id="medalhas-container">
        <h2>Medalhas e Troféus</h2>
        <div id="medalhas"></div>
    </div>

    <script>
        function selecionarAvatar(avatar) {
            document.querySelectorAll('.avatar').forEach(av => av.classList.remove('avatar-selected'));
            avatar.classList.add('avatar-selected');
        }

        function iniciarJogo() {
            document.getElementById("inicio").classList.add("fade-out");
            setTimeout(() => {
                document.getElementById("inicio").style.display = "none";
                document.getElementById("pista-container").style.display = "block";
                mostrarPista();
            }, 1000);
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = "🌉 Uma ponte que une passado e presente, iluminando noites românticas. Onde estou?";
            document.getElementById("mensagem").textContent = "Vá até a Ponte Hercílio Luz e clique no botão abaixo!";
        }

        function verificarLocalizacao() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition((position) => {
                    let distancia = calcularDistancia(position.coords.latitude, position.coords.longitude, -27.5973, -48.5515);
                    if (distancia < 0.2) {
                        desbloquearProximaPista();
                    } else {
                        alert("Você ainda não chegou ao local certo!");
                    }
                });
            } else {
                alert("Geolocalização não suportada no seu navegador.");
            }
        }

        function calcularDistancia(lat1, lon1, lat2, lon2) {
            const R = 6371;
            const dLat = (lat2 - lat1) * Math.PI / 180;
            const dLon = (lon2 - lon1) * Math.PI / 180;
            const a = Math.sin(dLat/2) * Math.sin(dLat/2) + Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLon/2) * Math.sin(dLon/2);
            return R * (2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)));
        }

        function desbloquearProximaPista() {
            alert("Parabéns! Você encontrou a pista correta!");
            ganharMedalha("Primeira Pista");
        }

        function ganharMedalha(nome) {
            const medalha = document.createElement("div");
            medalha.className = "medalha";
            medalha.style.backgroundImage = "url(https://example.com/medalha.png)";
            document.getElementById("medalhas").appendChild(medalha);
        }
    </script>
</body>
</html>
