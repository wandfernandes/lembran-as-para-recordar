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
        .heart {
            position: absolute;
            width: 50px;
            height: 50px;
            background: url('https://example.com/heart.png') no-repeat center center;
            background-size: cover;
            animation: float 5s infinite;
            opacity: 0.8;
        }
        @keyframes float {
            0% { transform: translateY(0); }
            50% { transform: translateY(-100px); }
            100% { transform: translateY(0); }
        }
    </style>
</head>
<body>
    <div id="inicio">
        <h1>🌸 Caça ao Tesouro - Florianópolis 🌸</h1>
        <p>Escolha seu avatar romântico:</p>
        <div class="avatar-selection">
            <img src="assets/images/avatar1.png" class="avatar" alt="Avatar 1" onclick="selecionarAvatar(this)">
            <img src="assets/images/avatar2.png" class="avatar" alt="Avatar 2" onclick="selecionarAvatar(this)">
            <img src="assets/images/avatar3.png" class="avatar" alt="Avatar 3" onclick="selecionarAvatar(this)">
        </div>
        <p>Insira sua chave de acesso:</p>
        <input type="text" id="chave" placeholder="Digite sua chave">
        <p>Selecione o nível de dificuldade:</p>
        <select id="nivelDificuldade">
            <option value="facil">Fácil</option>
            <option value="medio">Médio</option>
            <option value="dificil">Difícil</option>
        </select>
        <button onclick="iniciarJogo()">Começar</button>
    </div>

    <div id="pista-container">
        <h2 id="pista"></h2>
        <p id="mensagem"></p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
        <p id="score">Pontuação: 0</p>
    </div>

    <div id="medalhas-container">
        <h2>Medalhas e Troféus</h2>
        <div id="medalhas"></div>
    </div>

    <!-- Adicionando sons -->
    <audio id="somCorreto" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="somIncorreto" src="https://www.soundjay.com/button/beep-10.wav"></audio>
    <audio id="musicaFundo" src="https://www.soundjay.com/nature/sounds/rain-01.mp3" loop></audio>

    <script>
        const pistasOriginais = [
            { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui. Onde estou?", latitude: -27.5969, longitude: -48.4846, nome: "Lagoa da Conceição" },
            { charada: "🌉 Uma ponte que une passado e presente, iluminando noites românticas. Onde estou?", latitude: -27.5973, longitude: -48.5515, nome: "Ponte Hercílio Luz" },
            { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento. Um encontro perfeito. Onde estou?", latitude: -27.6206, longitude: -48.4354, nome: "Dunas da Joaquina" },
            { charada: "🏖️ Um paraíso de luxo e diversão onde o pôr do sol é digno de aplausos. Onde estou?", latitude: -27.4368, longitude: -48.4916, nome: "Praia de Jurerê" },
            { charada: "🍽️ Frutos do mar, cultura e encontros românticos entre as mesas. Onde estou?", latitude: -27.5951, longitude: -48.5480, nome: "Mercado Público" },
            { charada: "🌅 No alto da ilha, uma vista que revela toda a beleza de Floripa. Onde estou?", latitude: -27.5888, longitude: -48.5350, nome: "Mirante do Morro da Cruz" }
        ];

        let pistas = [];
        let indiceAtual = 0;
        let score = 0;
        let avatarSelecionado = '';
        let dificuldade = 'medio';

        function selecionarAvatar(avatar) {
            avatarSelecionado = avatar.alt;
            const avatares = document.querySelectorAll('.avatar');
            avatares.forEach(av => av.classList.remove('avatar-selected'));
            avatar.classList.add('avatar-selected');
        }

        function iniciarJogo() {
            let chave = document.getElementById("chave").value.trim();
            dificuldade = document.getElementById("nivelDificuldade").value;

            if (chave === "") {
                alert("Digite uma chave válida!");
                return;
            }

            pistas = embaralharPistas(chave);

            document.getElementById("inicio").classList.add("fade-out");
            setTimeout(() => {
                document.getElementById("inicio").style.display = "none";
                document.getElementById("pista-container").style.display = "block";
                mostrarPista();
                document.getElementById("musicaFundo").play();
                criarCorações();
            }, 1000);
        }

        function embaralharPistas(chave) {
            let seed = hashString(chave);
            let pistasEmbaralhadas = [...pistasOriginais];

            for (let i = pistasEmbaralhadas.length - 1; i > 0; i--) {
                let j = seed % (i + 1);
                [pistasEmbaralhadas[i], pistasEmbaralhadas[j]] = [pistasEmbaralhadas[j], pistasEmbaralhadas[i]];
                seed = (seed * 33) % 1000003;
            }

            return pistasEmbaralhadas;
        }

        function hashString(str) {
            let hash = 0;
            for (let i = 0; i < str.length; i++) {
                hash = (hash * 31 + str.charCodeAt(i)) % 1000003;
            }
            return hash;
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("mensagem").textContent = `Vá até ${pistas[indiceAtual].nome} e clique no botão abaixo!`;
            mostrarMapa(pistas[indiceAtual].latitude, pistas[indiceAtual].longitude);
        }

        function verificarLocalizacao() {
            const latUsuario = parseFloat(prompt("Digite a latitude atual:"));
            const longUsuario = parseFloat(prompt("Digite a longitude atual:"));
            const latPista = pistas[indiceAtual].latitude;
            const longPista = pistas[indiceAtual].longitude;

            const distancia = calcularDistancia(latUsuario, longUsuario, latPista, longPista);

            if (distancia < 0.2) {
                desbloquearProximaPista();
            } else {
                document.getElementById("mensagem").textContent = "📍 Você ainda não chegou ao local certo! Continue procurando!";
                const somIncorreto = document.getElementById("somIncorreto");
                somIncorreto.play();
            }
        }

        function mostrarMapa(lat, long) {
            document.getElementById("mapa").innerHTML = `<iframe width="100%" height="300" frameborder="0" src="https://www.google.com/maps?q=${lat},${long}&output=embed"></iframe>`;
        }

        function calcularDistancia(lat1, lon1, lat2, lon2) {
            const R = 6371;
            const dLat = (lat2 - lat1) * Math.PI / 180;
            const dLon = (lon2 - lon1) * Math.PI / 180;
            const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
                      Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
                      Math.sin(dLon / 2) * Math.sin(dLon / 2);
            return R * (2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a)));
        }

        function desbloquearProximaPista() {
            const somCorreto = document.getElementById("somCorreto");
            somCorreto.play();
            document.getElementById("mensagem").textContent = `Parabéns! Você encontrou a próxima pista!`;
            indiceAtual++;
            if (indiceAtual < pistas.length) {
                setTimeout(() => {
                    mostrarPista();
                    document.getElementById("mensagem").textContent = "";
                }, 3000);
            } else {
                document.getElementById("pista").textContent = "🎉 Parabéns! Você encontrou o tesouro romântico! 🎁";
                document.getElementById("mensagem").textContent = "";
            }
        }

        function criarCorações() {
            const numCoracoes = 20;
            for (let i = 0; i < numCoracoes; i++) {
                const coracao = document.createElement('div');
                coracao.className = 'heart';
                coracao.style.left = `${Math.random() * 100}vw`;
                coracao.style.animationDuration = `${Math.random() * 5 + 5}s`;
                document.body.appendChild(coracao);
            }
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
