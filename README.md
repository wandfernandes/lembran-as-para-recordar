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
        }
        h1 {
            font-family: 'Dancing Script', cursive;
            font-size: 3em;
            margin-top: 20px;
        }
        #inicio, #pista-container, #medalhas-container {
            margin-top: 50px;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div id="inicio">
        <h1>🌸 Caça ao Tesouro - Florianópolis 🌸</h1>
        <p>Insira sua chave de acesso:</p>
        <input type="text" id="chave" placeholder="Digite sua chave">
        <button onclick="iniciarJogo()">Começar</button>
    </div>

    <div id="pista-container" class="hidden">
        <h2 id="pista"></h2>
        <p id="descricao"></p>
        <button onclick="verificarLocalizacao()">Verificar Localização</button>
        <div id="mapa"></div>
        <p id="score">Pontuação: 0</p>
    </div>

    <script>
        const pistasOriginais = [
            { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui. Onde estou?", lat: -27.5969, lon: -48.4846, descricao: "A Lagoa da Conceição é um dos locais mais românticos de Florianópolis. É perfeita para passeios de pedalinho, caiaque e jantares à beira da água." },
            { charada: "🌉 Uma ponte que une passado e presente. Onde estou?", lat: -27.5973, lon: -48.5515, descricao: "A Ponte Hercílio Luz é um dos cartões-postais da cidade. Construída em 1926, foi a primeira ligação entre a Ilha e o continente." },
            { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento. Onde estou?", lat: -27.6206, lon: -48.4354, descricao: "As Dunas da Joaquina são ideais para o sandboard. O local atrai aventureiros e turistas que querem se divertir deslizando pela areia." },
            { charada: "🏖️ Um paraíso de luxo e diversão. Onde estou?", lat: -27.4368, lon: -48.4916, descricao: "Jurerê Internacional é famosa por suas praias de areia branca, bares sofisticados e festas badaladas." },
            { charada: "🐚 Um recanto de águas calmas e piscinas naturais, perfeito para um mergulho. Onde estou?", lat: -27.3285, lon: -48.5446, descricao: "A Praia do Forte encanta com suas águas tranquilas e suas ruínas históricas do século XVIII." },
            { charada: "🌴 Um refúgio escondido entre trilhas e natureza preservada. Onde estou?", lat: -27.6424, lon: -48.4334, descricao: "Lagoinha do Leste é uma praia selvagem, acessível apenas por trilha ou barco, conhecida por sua beleza intocada." },
            { charada: "🍤 Um vilarejo encantador, conhecido por seu pôr do sol e frutos do mar. Onde estou?", lat: -27.7144, lon: -48.5000, descricao: "Santo Antônio de Lisboa tem uma das vistas mais bonitas do pôr do sol e é famoso por seus restaurantes de frutos do mar." },
            { charada: "🚣‍♀️ Uma ilha onde o tempo parece ter parado, com casas coloniais e ruas de terra. Onde estou?", lat: -27.7487, lon: -48.5645, descricao: "A Ilha de Anhatomirim abriga uma fortaleza histórica e é acessível por passeios de barco, sendo um refúgio de tranquilidade e história." },
            { charada: "🐳 Um santuário ecológico onde baleias francas visitam no inverno. Onde estou?", lat: -27.8565, lon: -48.5608, descricao: "A Praia do Rosa é um dos melhores lugares para observar baleias no inverno, além de ser um destino famoso para casais e surfistas." }
        ];

        let pistas = [], indiceAtual = 0, score = 0;

        function iniciarJogo() {
            let chave = document.getElementById("chave").value.trim();
            if (!chave) return alert("Digite uma chave válida!");

            pistas = embaralharPistas(chave);
            document.getElementById("inicio").classList.add("hidden");
            document.getElementById("pista-container").classList.remove("hidden");
            mostrarPista();
        }

        function embaralharPistas(chave) {
            let seed = chave.split("").reduce((acc, char) => acc + char.charCodeAt(0), 0);
            return [...pistasOriginais].sort(() => (seed = (seed * 33) % 1000003) % 2 ? 1 : -1);
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("descricao").textContent = pistas[indiceAtual].descricao;
            mostrarMapa(pistas[indiceAtual].lat, pistas[indiceAtual].lon);
        }

        function verificarLocalizacao() {
            if (!navigator.geolocation) return alert("Geolocalização não suportada!");

            navigator.geolocation.getCurrentPosition(posicao => {
                const { latitude, longitude } = posicao.coords;
                const { lat, lon } = pistas[indiceAtual];
                if (calcularDistancia(latitude, longitude, lat, lon) < 0.2) desbloquearProximaPista();
                else alert("📍 Você ainda não chegou ao local correto!");
            }, () => alert("Erro ao obter localização. Verifique as permissões."));
        }

        function desbloquearProximaPista() {
            score += 10;
            document.getElementById("score").textContent = `Pontuação: ${score}`;
            alert("✅ Você encontrou o local! Vamos para o próximo!");

            if (++indiceAtual < pistas.length) mostrarPista();
            else alert("🎉 Parabéns! Você completou a caça ao tesouro!");
        }

        function mostrarMapa(lat, lon) {
            document.getElementById("mapa").innerHTML = `<iframe width="100%" height="300" src="https://www.google.com/maps?q=${lat},${lon}&output=embed"></iframe>`;
        }

        function calcularDistancia(lat1, lon1, lat2, lon2) {
            const R = 6371, dLat = (lat2 - lat1) * Math.PI / 180, dLon = (lon2 - lon1) * Math.PI / 180;
            return R * (2 * Math.atan2(Math.sqrt(Math.sin(dLat / 2) ** 2 + Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLon / 2) ** 2), Math.sqrt(1 - Math.sin(dLat / 2) ** 2 - Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * Math.sin(dLon / 2) ** 2)));
        }
    </script>
</body>
</html>
