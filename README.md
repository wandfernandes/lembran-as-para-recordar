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
        #assistente {
            background: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
        }
        #score-board {
            display: flex;
            justify-content: space-around;
            padding: 10px;
        }
        .medal {
            background: gold;
            padding: 10px;
            border-radius: 5px;
            font-weight: bold;
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
        <button onclick="verProximaPista()">Próxima Pista</button>
        <p id="score">Pontuação: 0</p>

        <div id="assistente">
            <h3>💡 Assistente Virtual 💡</h3>
            <p id="dica">Clique no botão para receber uma dica!</p>
            <button onclick="fornecerDica()">Receber Dica</button>
        </div>

        <div id="score-board">
            <div class="medal" id="medalha-historico">🏅 Medalha Histórica</div>
            <div class="medal" id="medalha-natureza">🏅 Medalha Aventureiro</div>
        </div>
    </div>

    <script>
        let pistas = [
            { charada: "🌊 Um espelho d’água cercado por dunas e natureza. Casais adoram remar aqui.", descricao: "Lagoa da Conceição: Passeios de pedalinho, bares charmosos e uma vista incrível!" },
            { charada: "🌉 Uma ponte que une passado e presente.", descricao: "Ponte Hercílio Luz: O maior cartão-postal de Florianópolis, inaugurado em 1926." },
            { charada: "🏄‍♂️ Dunas douradas onde aventureiros deslizam ao vento.", descricao: "Dunas da Joaquina: Um dos melhores lugares para sandboard no Brasil." },
            { charada: "🏖️ Um paraíso de luxo e diversão.", descricao: "Jurerê Internacional: Conhecida por festas sofisticadas e praia de águas cristalinas." },
            { charada: "🐚 Um recanto de águas calmas e piscinas naturais, perfeito para um mergulho.", descricao: "Praia do Forte: Local tranquilo, com águas cristalinas e ruínas históricas." },
            { charada: "🌴 Um refúgio escondido entre trilhas e natureza preservada.", descricao: "Lagoinha do Leste: Praia selvagem acessível apenas por trilha ou barco." },
            { charada: "🍤 Um vilarejo encantador, conhecido por seu pôr do sol e frutos do mar.", descricao: "Santo Antônio de Lisboa: Um dos melhores locais para apreciar o pôr do sol." },
            { charada: "🚣‍♀️ Uma ilha onde o tempo parece ter parado, com casas coloniais e ruas de terra.", descricao: "Ilha de Anhatomirim: Abriga uma fortaleza do século XVIII, acessível por barco." },
            { charada: "🐳 Um santuário ecológico onde baleias francas visitam no inverno.", descricao: "Praia do Rosa: Um dos melhores pontos de observação de baleias no Brasil." }
        ];

        let indiceAtual = 0, score = 0;
        let medalhas = { historico: false, natureza: false };

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
            let shuffled = [...pistas];
            for (let i = shuffled.length - 1; i > 0; i--) {
                let j = Math.floor((seed * (i + 1)) % shuffled.length);
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
            }
            return shuffled;
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("descricao").textContent = pistas[indiceAtual].descricao;
        }

        function verProximaPista() {
            score += 10;
            document.getElementById("score").textContent = `Pontuação: ${score}`;

            if (indiceAtual === 0) {
                medalhas.historico = true;
                document.getElementById("medalha-historico").style.display = "block";
            }
            if (indiceAtual === 5) {
                medalhas.natureza = true;
                document.getElementById("medalha-natureza").style.display = "block";
            }

            if (++indiceAtual < pistas.length) {
                mostrarPista();
            } else {
                alert("🎉 Parabéns! Você completou a caça ao tesouro!");
                document.getElementById("pista-container").classList.add("hidden");
                document.getElementById("inicio").classList.remove("hidden");
            }
        }

        function fornecerDica() {
            let dicas = [
                "Preste atenção nos detalhes da pista! Alguma palavra-chave pode ajudar.",
                "Pense em locais turísticos famosos de Florianópolis.",
                "Se a pista menciona água, pode ser uma praia, lagoa ou ponte!",
                "Algumas pistas fazem referência a locais históricos.",
                "Se o local tem trilha, pode ser uma praia mais isolada!",
                "Releia a charada e tente associar com lugares conhecidos da cidade."
            ];
            let dicaAleatoria = dicas[Math.floor(Math.random() * dicas.length)];
            document.getElementById("dica").textContent = dicaAleatoria;
        }
    </script>
</body>
</html>
