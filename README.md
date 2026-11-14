<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>✨ Pega as Estrelinhas!</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: sans-serif;
            touch-action: manipulation;
        }

        body {
            background: linear-gradient(to bottom, #1a1a2e, #16213e);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 10px;
            overflow: hidden;
        }

        /* Tela Inicial */
        .tela-inicial {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, #1a1a2e, #16213e);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 100;
            padding: 20px;
        }

        .capa {
            background: rgba(255, 255, 255, 0.05);
            border: 3px solid gold;
            border-radius: 20px;
            padding: 40px 30px;
            max-width: 500px;
            width: 100%;
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.3);
            text-align: center;
        }

        h1 {
            color: #ffd700;
            font-size: 32px;
            margin-bottom: 10px;
            text-shadow: 0 0 15px gold;
        }

        .subtitulo {
            color: #aaa;
            font-size: 18px;
            margin-bottom: 30px;
        }

        .opcoes {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .botao {
            display: block;
            text-decoration: none;
            padding: 18px;
            border-radius: 12px;
            font-size: 20px;
            font-weight: bold;
            transition: 0.3s;
            border: 2px solid transparent;
            cursor: pointer;
        }

        .botao-celular {
            background: rgba(0, 255, 255, 0.15);
            color: #00ffff;
            border-color: #00ffff;
        }
        .botao-celular:hover {
            background: #00ffff;
            color: #111;
            box-shadow: 0 0 20px #00ffff;
        }

        .botao-pc {
            background: rgba(255, 215, 0, 0.15);
            color: #ffd700;
            border-color: #ffd700;
        }
        .botao-pc:hover {
            background: #ffd700;
            color: #111;
            box-shadow: 0 0 20px gold;
        }

        .icone { font-size: 24px; margin-right: 8px; }
        .dica { margin-top: 25px; color: #777; font-size: 14px; }

        /* Tela do Jogo */
        .tela-jogo { display: none; width: 100%; flex-direction: column; align-items: center; }
        .escondida { display: none !important; }

        .placar { color: white; font-size: 20px; margin: 10px 0; font-weight: bold; }
        
        canvas {
            border: 3px solid gold;
            border-radius: 8px;
            background: url('https://cdn.pixabay.com/photo/2017/08/30/01/05/milky-way-2695569_1280.jpg');
            background-size: cover;
            max-width: 100%;
            height: auto;
        }

        /* Botões Mobile */
        .botoes {
            margin-top: 20px;
            display: grid;
            grid-template-columns: repeat(3, 80px);
            grid-template-rows: repeat(2, 80px);
            gap: 8px;
        }
        .botao-controle {
            width: 80px;
            height: 80px;
            background: rgba(0, 255, 255, 0.2);
            border: 2px solid #00ffff;
            border-radius: 50%;
            color: white;
            font-size: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            user-select: none;
            -webkit-user-select: none;
        }
        .botao-controle:active {
            background: rgba(0, 255, 255, 0.5);
            transform: scale(0.95);
        }
        .vazio { visibility: hidden; }

        .controles-pc { color: #aaa; margin-top: 15px; text-align: center; }
        .btn-voltar {
            margin-top: 15px;
            padding: 10px 20px;
            background: rgba(255,255,255,0.1);
            border: 1px solid #666;
            color: white;
            border-radius: 8px;
            cursor: pointer;
        }
        .btn-voltar:hover { background: rgba(255,255,255,0.2); }

        @media (max-width: 480px) {
            h1 { font-size: 24px; }
            .botao { font-size: 18px; padding: 15px; }
            .capa { padding: 30px 20px; }
        }
    </style>
</head>
<body>

    <!-- Tela Inicial -->
    <div class="tela-inicial" id="telaInicial">
        <div class="capa">
            <h1>✨ PEGA AS ESTRELINHAS! ✨</h1>
            <p class="subtitulo">Escolha como você quer jogar:</p>

            <div class="opcoes">
                <button class="botao botao-celular" onclick="iniciarJogo('mobile')">
                    <span class="icone">📱</span> Jogar no Celular
                </button>
                <button class="botao botao-pc" onclick="iniciarJogo('pc')">
                    <span class="icone">💻</span> Jogar no Computador
                </button>
            </div>

            <p class="dica">
                No celular: botões na tela<br>
                No PC: setas do teclado
            </p>
        </div>
    </div>

    <!-- Tela do Jogo -->
    <div class="tela-jogo escondida" id="telaJogo">
        <h1>✨ PEGA AS ESTRELINHAS!</h1>
        <div class="placar">Pontos: <span id="pontos">0</span></div>
        <canvas id="tela" width="360" height="300"></canvas>

        <!-- Controles Mobile -->
        <div class="botoes escondida" id="controlesMobile">
            <div class="vazio"></div>
            <div class="botao-controle" id="btnCima">⬆️</div>
            <div class="vazio"></div>
            <div class="botao-controle" id="btnEsq">⬅️</div>
            <div class="vazio"></div>
            <div class="botao-controle" id="btnDir">➡️</div>
            <div class="vazio"></div>
            <div class="botao-controle" id="btnBaixo">⬇️</div>
            <div class="vazio"></div>
        </div>

        <!-- Controles PC -->
        <div class="controles-pc escondida" id="controlesPC">
            ⬅️ ➡️ ⬆️ ⬇️ Use as setas do teclado para mover
        </div>

        <button class="btn-voltar" onclick="voltarParaInicio()">⬅️ Voltar</button>
    </div>

    <script>
        const telaInicial = document.getElementById('telaInicial');
        const telaJogo = document.getElementById('telaJogo');
        const controlesMobile = document.getElementById('controlesMobile');
        const controlesPC = document.getElementById('controlesPC');
        const canvas = document.getElementById('tela');
        const ctx = canvas.getContext('2d');
        const pontosEl = document.getElementById('pontos');

        let jogador, estrelas, pontos, teclas, loopAtivo;

        function iniciarJogo(modo) {
            telaInicial.classList.add('escondida');
            telaJogo.classList.remove('escondida');
            telaJogo.style.display = 'flex';

            // Ajustar tamanho do canvas conforme o modo
            if (modo === 'pc') {
                controlesMobile.classList.add('escondida');
                controlesPC.classList.remove('escondida');
                canvas.width = 600;
                canvas.height = 400;
                jogador = { x: 280, y: 180, largura: 40, altura: 40, velocidade: 5 };
            } else {
                controlesMobile.classList.remove('escondida');
                controlesPC.classList.add('escondida');
                canvas.width = 360;
                canvas.height = 300;
                jogador = { x: 160, y: 130, largura: 30, altura: 30, velocidade: 4 };
                configurarBotoesMobile();
            }

            estrelas = [];
            pontos = 0;
            teclas = { cima: false, baixo: false, esquerda: false, direita: false };
            pontosEl.textContent = '0';

            // Teclado funciona nos dois modos
            document.addEventListener('keydown', e => {
                if(e.key === 'ArrowUp') teclas.cima = true;
                if(e.key === 'ArrowDown') teclas.baixo = true;
                if(e.key === 'ArrowLeft') teclas.esquerda = true;
                if(e.key === 'ArrowRight') teclas.direita = true;
            });
            document.addEventListener('keyup', e => {
                if(e.key === 'ArrowUp') teclas.cima = false;
                if(e.key === 'ArrowDown') teclas.baixo = false;
                if(e.key === 'ArrowLeft') teclas.esquerda = false;
                if(e.key === 'ArrowRight') teclas.direita = false;
            });

            for(let i = 0; i < 5; i++) criarEstrela();
            loopAtivo = true;
            loop();
        }

        function configurarBotoesMobile() {
            function botao(id, dir) {
                const btn = document.getElementById(id);
                btn.addEventListener('touchstart', e => { e.preventDefault(); teclas[dir] = true; });
                btn.addEventListener('touchend', e => { e.preventDefault(); teclas[dir] = false; });
                btn.addEventListener('mousedown', () => teclas[dir] = true);
                btn.addEventListener('mouseup', () => teclas[dir] = false);
                btn.addEventListener('mouseleave', () => teclas[dir] = false);
            }
            botao('btnCima', 'cima');
            botao('btnBaixo', 'baixo');
            botao('btnEsq', 'esquerda');
            botao('btnDir', 'direita');
        }

        function voltarParaInicio() {
            loopAtivo = false;
            telaJogo.classList.add('escondida');
            telaInicial.classList.remove('escondida');
        }

        function criarEstrela() {
            estrelas.push({
                x: Math.random() * (canvas.width - 30),
                y: Math.random() * (canvas.height - 30),
                tamanho: 25
            });
        }

        function atualizar() {
            if(teclas.cima && jogador.y > 0) jogador.y -= jogador.velocidade;
            if(teclas.baixo && jogador.y < canvas.height - jogador.altura) jogador.y += jogador.velocidade;
            if(teclas.esquerda && jogador.x > 0) jogador.x -= jogador.velocidade;
            if(teclas.direita && jogador.x < canvas.width - jogador.largura) jogador.x += jogador.velocidade;

            estrelas.forEach((estrela, indice) => {
                if (
                    jogador.x < estrela.x + estrela.tamanho &&
                    jogador.x + jogador.largura > estrela.x &&
                    jogador.y < estrela.y + estrela.tamanho &&
                    jogador.y + jogador.altura > estrela.y
                ) {
                    estrelas.splice(indice, 1);
                    pontos++;
                    pontosEl.textContent = pontos;
                    criarEstrela();
                    if(pontos % 5 === 0) criarEstrela();
                }
            });
        }

        function desenhar() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Nave do jogador
            ctx.fillStyle = '#00ffff';
            ctx.shadowBlur = 10;
            ctx.shadowColor = '#00ffff';
            ctx.beginPath();
            ctx.moveTo(jogador.x + jogador.largura/2, jogador.y);
            ctx.lineTo(jogador.x, jogador.y + jogador.altura);
            ctx.lineTo(jogador.x + jogador.largura, jogador.y + jogador.altura);
            ctx.closePath();
            ctx.fill();
            ctx.shadowBlur = 0;

            // Estrelas
            ctx.fillStyle = '#ffd700';
            estrelas.forEach(e => desenharEstrela(e.x + e.tamanho/2, e.y + e.tamanho/2, 5, e.tamanho/2, e.tamanho/4));
        }

        function desenharEstrela(cx, cy, pontas, rExt, rInt) {
            let rot = Math.PI / 2 * 3;
            let passo = Math.PI / pontas;
            ctx.beginPath();
            ctx.moveTo(cx, cy - rExt);
            for (let i = 0; i < pontas; i++) {
                ctx.lineTo(cx + Math.cos(rot)*rExt, cy + Math.sin(rot)*rExt);
                rot += passo;
                ctx.lineTo(cx + Math.cos(rot)*rInt, cy + Math.sin(rot)*rInt);
                rot += passo;
            }
            ctx.lineTo(cx, cy - rExt);
            ctx.closePath();
            ctx.fill();
        }

        function loop() {
            if (!loopAtivo) return;
            atualizar();
            desenhar();
            requestAnimationFrame(loop);
        }
    </script>
</body>
</html>
