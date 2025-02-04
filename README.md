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
        #cronometro {
            font-size: 1.5em;
            margin-top: 20px;
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
        <div id="cronometro">Tempo restante: 00:00</div>

        <div id="assistente">
            <h3>💡 Assistente Virtual 💡</h3>
            <p id="dica">Clique no botão para receber uma dica!</p>
            <button onclick="fornecerDica()">Receber Dica</button>
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

        let indiceAtual = 0, score = 0, moedas = 0;
        let tempoRestante = 300;  // 5 minutos por pista
        let cronometroInterval;

        function iniciarJogo() {
            let chave = document.getElementById("chave").value.trim();
            if (!chave) return alert("Digite uma chave válida!");

            pistas = embaralharPistas(chave);
            document.getElementById("inicio").classList.add("hidden");
            document.getElementById("pista-container").classList.remove("hidden");
            mostrarPista();
            iniciarCronometro();
        }

        function embaralharPistas(chave) {
            let seed = chave.split("").reduce((acc, char) => acc + char.charCodeAt(0), 0);
            return [...pistas].sort(() => (seed = (seed * 33) % 1000003) % 2 ? 1 : -1);
        }

        function mostrarPista() {
            document.getElementById("pista").textContent = pistas[indiceAtual].charada;
            document.getElementById("descricao").textContent = pistas[indiceAtual].descricao;
        }

        function iniciarCronometro() {
            cronometroInterval = setInterval(() => {
                if (tempoRestante <= 0) {
                    clearInterval(cronometroInterval);
                    alert("Tempo esgotado! Você perdeu a oportunidade de continuar.");
                } else {
                    tempoRestante--;
                    let minutos = Math.floor(tempoRestante / 60);
                    let segundos = tempoRestante % 60;
                    document.getElementById("cronometro").textContent = `Tempo restante: ${minutos}:${segundos < 10 ? '0' : ''}${segundos}`;
                }
            }, 1000);
        }

        function verProximaPista() {
            score += 10;
            moedas += 5;
            document.getElementById("score").textContent = `Pontuação: ${score}`;
            document.getElementById("moedas").textContent = `Moedas: ${moedas}`;

            if (++indiceAtual < pistas.length) {
                mostrarPista();
                tempoRestante = 300; // Resetando o tempo para 5 minutos na próxima pista
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

        // Função para pedir foto e validar
        function pedirFoto() {
            let fotoTirada = prompt("Tire uma foto do local e digite o nome do arquivo da foto para prosseguir.");
            if (fotoTirada) {
                alert("Foto recebida! Avançando para a próxima pista.");
            } else {
                alert("Não foi possível validar a foto. Tente novamente.");
            }
        }
    </script>
</body>
</html>
