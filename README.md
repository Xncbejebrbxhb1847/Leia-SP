Leia-me Basic Cheat

Um cheat simples e eficiente para o [Leia-me Basic](https://example.com) desenvolvido com Tampermonkey. Este projeto visa facilitar a experiência dos usuários em quizzes, automatizando respostas e melhorando a eficiência dos estudos.

🚀 Começando

Essas instruções irão ajudá-lo a obter uma cópia do projeto em execução na sua máquina local para desenvolvimento e testes.

📋 Pré-requisitos

- [Tampermonkey](https://www.tampermonkey.net/) (extensão de navegador)
- Acesso à internet

🔧 Instalação

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/Xncbejebrbxhb1847/leia-me-basic-cheat.git

2. Instale o script no Tampermonkey:

Abra o Tampermonkey em seu navegador.

Clique em "Criar um novo script".

Cole o código do script leia-me-basic-cheat na nova janela e salve.



---

🛠️ Funcionalidades

Iniciar e parar o cheat: Controle total sobre a automação do cheat.

Configuração de tempos: Ajuste o intervalo de respostas para uma experiência personalizada.

Design responsivo: Funciona perfeitamente em dispositivos móveis e desktops.

---

🤝 Contribuições

As contribuições são bem-vindas! Se você tiver sugestões ou melhorias, sinta-se à vontade para abrir uma issue ou enviar um pull request.

1. Faça um fork do projeto.


2. Crie sua branch: git checkout -b feature/YourFeature.


3. Commit suas mudanças: git commit -m 'Adicionando uma nova feature'.


4. Faça um push: git push origin feature/YourFeature.


5. Abra um pull request.




---

📞 Contato

GitHub: Xncbejebrbxhb1847

Discord: https://discord.gg/jv8JPtFS



---

📄 Licença

Este projeto está licenciado sob a licença MIT - consulte o arquivo LICENSE para detalhes.


---

📝 Código do Script

// ==UserScript==
// @name         Leia-me basic cheat
// @namespace    http://tampermonkey.net/
// @version      2.3
// @description  Cheat para leia-me basic
// @author       iUnknown owner // update by wyzop__
// @match        *://*/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    let count = 0;
    let maxTimes = 300;
    let intervalId;
    let isRunning = false;
    let detectQuiz = true;
    let autoContinueAfterQuiz = false;
    let intervalTime = 1000;
    let quizCheckIntervalId = null;

    const style = document.createElement('style');
    style.textContent = `
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

        :root {
            --primary-color: #4a90e2;
            --secondary-color: #f39c12;
            --background-color: #2c3e50;
            --text-color: #ecf0f1;
            --success-color: #2ecc71;
            --danger-color: #e74c3c;
            --input-background: #34495e;
            --hover-color: #3498db;
        }

        .leiacheat-bar {
            position: fixed;
            top: 20px;
            right: 20px;
            color: var(--text-color);
            padding: 15px 20px;
            border-radius: 10px;
            font-family: 'Roboto', sans-serif;
            z-index: 10000;
            cursor: pointer;
            transition: all 0.3s ease;
            background-color: rgba(74, 144, 226, 0.8);
            border: 2px solid var(--primary-color);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .leiacheat-bar:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(74, 144, 226, 0.4);
            background-color: rgba(74, 144, 226, 1);
        }

        .leiacheat-bar h3 {
            margin: 0;
            font-size: 18px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .leiacheat-menu {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: var(--background-color);
            color: var(--text-color);
            padding: 25px;
            border-radius: 15px;
            font-family: 'Roboto', sans-serif;
            z-index: 10000;
            box-shadow: 0 8px 30px rgba(0,0,0,0.4);
            width: 90%;
            max-width: 350px;
            max-height: 90vh;
            overflow-y: auto;
        }

        @media (max-width: 768px) {
            .leiacheat-menu {
                width: 95%;
                padding: 15px;
                font-size: 16px;
            }

            .leiacheat-menu button,
            .leiacheat-menu input[type="number"] {
                font-size: 16px;
                padding: 15px;
                height: 50px;
            }

            .leiacheat-menu-header h2 {
                font-size: 20px;
            }

            .leiacheat-menu-section h3 {
                font-size: 18px;
            }

            .leiacheat-menu label {
                font-size: 16px;
            }

            .leiacheat-tooltip .leiacheat-tooltiptext {
                font-size: 14px;
                width: 250px;
                margin-left: -125px;
            }

            .leiacheat-menu-footer {
                font-size: 14px;
            }

            .leiacheat-bar h3 {
                font-size: 16px;
            }

            .leiacheat-notification {
                font-size: 14px;
                padding: 12px 20px;
            }
        }

        .leiacheat-menu::-webkit-scrollbar {
            width: 10px;
        }

        .leiacheat-menu::-webkit-scrollbar-track {
            background: var(--background-color);
            border-radius: 10px;
        }

        .leiacheat-menu::-webkit-scrollbar-thumb {
            background: var(--primary-color);
            border-radius: 10px;
        }

        .leiacheat-menu::-webkit-scrollbar-thumb:hover {
            background: var(--hover-color);
        }

        .leiacheat-menu-header {
            text-align: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid var(--primary-color);
        }

        .leiacheat-menu-header h2 {
            font-size: 24px;
            margin: 0;
            color: var(--primary-color);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }

        .leiacheat-menu-section {
            margin-bottom: 25px;
            padding: 15px;
            background-color: rgba(52, 73, 94, 0.5);
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .leiacheat-menu-section h3 {
            font-size: 18px;
            margin-bottom: 15px;
            color: var(--secondary-color);
            border-bottom: 1px solid var(--secondary-color);
            padding-bottom: 5px;
        }

        .leiacheat-menu button {
            margin: 10px 0;
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 500;
            transition: all 0.3s ease;
            width: 100%;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        #start-btn {
            background-color: var(--success-color);
            color: white;
        }

        #start-btn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }

        #stop-btn {
            background-color: var(--danger-color);
            color: white;
        }

        #stop-btn:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }

        .leiacheat-menu button:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            filter: brightness(110%);
        }

        .leiacheat-menu button:active:not(:disabled) {
            transform: translateY(1px);
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .leiacheat-menu input[type="number"] {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            background-color: var(--input-background);
            color: var(--text-color);
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .leiacheat-menu input[type="number"]:focus {
            outline: none;
            box-shadow: 0 0 0 2px var(--primary-color);
        }

        .leiacheat-menu label {
            display: flex;
            align-items: center;
            margin: 15px 0;
            font-size: 14px;
            cursor: pointer;
        }

        .leiacheat-menu input[type="checkbox"] {
            margin-right: 10px;
            width: 18px;
            height: 18px;
            cursor: pointer;
            appearance: none;
            -webkit-appearance: none;
            background-color: var(--input-background);
            border: 2px solid var(--primary-color);
            border-radius: 4px;
            outline: none;
            transition: all 0.3s ease;
        }

        .leiacheat-menu input[type="checkbox"]:checked {
            background-color: var(--primary-color);
        }

        .leiacheat-menu input[type="checkbox"]:checked::before {
            content: '✓';
            display: block;
            text-align: center;
            color: var(--text-color);
            font-size: 14px;
            line-height: 18px;
        }

        .leiacheat-notification {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background-color: var(--primary-color);
            color: var(--text-color);
            padding: 15px 25px;
            border-radius: 10px;
            font-family: 'Roboto', sans-serif;
            z-index: 10001;
            opacity: 0;
            transition: all 0.3s ease-in-out;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            max-width: 300px;
        }

        .leiacheat-menu-footer {
            text-align: center;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 2px solid var(--primary-color);
            font-size: 12px;
            color: var(--text-color);
        }

        .leiacheat-menu-footer p {
            margin: 5px 0;
        }

        .leiacheat-tooltip {
            position: relative;
            display: inline-block;
            cursor: help;
        }

        .leiacheat-tooltip .leiacheat-tooltiptext {
            visibility: hidden;
            width: 200px;
            background-color: var(--background-color);
            color: var(--text-color);
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -100px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .leiacheat-tooltip:hover .leiacheat-tooltiptext {
            visibility: visible;
            opacity: 1;
        }

        .leiacheat-menu-section-content {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .leiacheat-input-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .leiacheat-input-group label {
            font-weight: bold;
        }

        .leiacheat-checkbox-group {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .leiacheat-checkbox-group label {
            font-weight: normal;
        }

        .leiacheat-instructions {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: var(--background-color);
            color: var(--text-color);
            padding: 25px;
            border-radius: 15px;
            font-family: 'Roboto', sans-serif;
            z-index: 10002;
            box-shadow: 0 8px 30px rgba(0,0,0,0.4);
            width: 90%;
            max-width: 400px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .leiacheat-instructions h2 {
            color: var(--primary-color);
            margin-bottom: 15px;
        }

        .leiacheat-instructions p {
            margin-bottom: 10px;
        }

        .leiacheat-instructions button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 15px;
        }

        .leiacheat-instructions button:hover {
            background-color: var(--hover-color);
        }

        @media (max-width: 768px) {
            body, input, button, select, textarea {
                font-family: 'Roboto', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', Arial, sans-serif !important;
            }

            .leiacheat-menu, .leiacheat-instructions, .leiacheat-notification {
                font-size: 16px !important;
                line-height: 1.5 !important;
            }

            .leiacheat-menu input[type="number"],
            .leiacheat-menu input[type="checkbox"],
            .leiacheat-menu label,
            .leiacheat-menu button {
                font-size: 16px !important;
            }

            .leiacheat-menu-header h2 {
                font-size: 22px !important;
            }

            .leiacheat-menu-section h3 {
                font-size: 20px !important;
            }

            .leiacheat-tooltip .leiacheat-tooltiptext {
                font-size: 14px !important;
            }

            .leiacheat-bar h3 {
                font-size: 18px !important;
            }
        }
    `;
    document.head.appendChild(style);

    function createBar() {
        const bar = document.createElement('div');
        bar.className = 'leiacheat-bar';
        bar.innerHTML = `
            <h3>LeiaCheat</h3>
        `;
        document.body.appendChild(bar);

        bar.addEventListener('click', toggleMenu);
    }

    function createMenu() {
        const menu = document.createElement('div');
        menu.className = 'leiacheat-menu';
        menu.innerHTML = `
            <div class="leiacheat-menu-header">
                <h2>LeiaCheat Configurações</h2>
            </div>

            <div class="leiacheat-menu-section">
                <h3>Controles</h3>
                <div class="leiacheat-menu-section-content">
                    <button id="start-btn">Iniciar</button>
                    <button id="stop-btn" disabled>Parar</button>
                </div>
            </div>

            <div class="leiacheat-menu-section">
                <h3>Configurações de Tempo</h3>
                <div class="leiacheat-menu-section-content">
                    <div class="leiacheat-input-group">
                        <label for="seconds-input">Tempo de passagem (segundos):</label>
                        <input type="number" id="seconds-input" min="1" step="1">
                    </div>
                    <div class="leiacheat-input-group">
                        <label for="milliseconds-input">Tempo de passagem (milissegundos):</label>
                        <input type="number" id="milliseconds-input" min="1" step="1">
                    </div>
                </div>
            </div>

            <div class="leiacheat-menu-section">
                <h3>Opções Adicionais</h3>
                <div class="leiacheat-menu-section-content">
                    <div class="leiacheat-checkbox-group">
                        <input type="checkbox" id="detect-quiz" checked>
                        <label for="detect-quiz">
                            Detectar quiz (atividade)
                            <span class="leiacheat-tooltip">ℹ️
                                <span class="leiacheat-tooltiptext">Com está opção ativada, irá parar antes de ativadades</span>
                            </span>
                        </label>
                    </div>
                    <div class="leiacheat-checkbox-group">
                        <input type="checkbox" id="auto-continue">
                        <label for="auto-continue">
                            Auto-continuar depois da atividade
                            <span class="leiacheat-tooltip">ℹ️
                                <span class="leiacheat-tooltiptext">Com esta opção ativada, irá continuar a passagem automaticamente depois do quiz sumir</span>
                            </span>
                        </label>
                    </div>
                </div>
            </div>

            <div class="leiacheat-menu-footer">
                <p>Desenvolvido por iUnknown owner</p>
                <p>Atualizado por wyzop__</p>
                <p>Versão 3.2</p>
            </div>
        `;
        document.body.appendChild(menu);

        document.getElementById('start-btn').addEventListener('click', startInterval);
        document.getElementById('stop-btn').addEventListener('click', stopInterval);
        document.getElementById('seconds-input').addEventListener('input', updateIntervalFromSeconds);
        document.getElementById('milliseconds-input').addEventListener('input', updateIntervalFromMilliseconds);
        document.getElementById('detect-quiz').addEventListener('change', (e) => {
            detectQuiz = e.target.checked;
        });
        document.getElementById('auto-continue').addEventListener('change', (e) => {
            autoContinueAfterQuiz = e.target.checked;
        });
    }

    function createInstructions() {
        const instructions = document.createElement('div');
        instructions.className = 'leiacheat-instructions';
        instructions.style.display = 'block'; // Garante que as instruções sejam visíveis inicialmente
        instructions.innerHTML = `
            <h2>Instruções de uso:</h2>
            <p>1. Clique no botão à direita escrito "LeiaCheat"</p>
            <p>2. Irá abrir um menu com funções autoexplicativas.</p>
            <button id="close-instructions-btn">Fechar instruções</button>
        `;
        document.body.appendChild(instructions);

        // Adiciona o evento de clique diretamente ao botão
        const closeButton = instructions.querySelector('#close-instructions-btn');
        closeButton.addEventListener('click', function() {
            instructions.style.display = 'none';
        });

        // Retorna o elemento de instruções
        return instructions;
    }

    function toggleMenu() {
        const menu = document.querySelector('.leiacheat-menu');
        menu.style.display = menu.style.display === 'none' ? 'block' : 'none';
    }

    function showNotification(message) {
        const notification = document.createElement('div');
        notification.className = 'leiacheat-notification';
        notification.textContent = message;
        document.body.appendChild(notification);

        setTimeout(() => {
            notification.style.opacity = '1';
            notification.style.transform = 'translateY(-20px)';
        }, 100);

        setTimeout(() => {
            notification.style.opacity = '0';
            notification.style.transform = 'translateY(0)';
            setTimeout(() => {
                document.body.removeChild(notification);
            }, 300);
        }, 3000);
    }

    function updateIntervalFromSeconds() {
        const seconds = document.getElementById('seconds-input').value;
        if (seconds) {
            intervalTime = seconds * 1000;
            document.getElementById('milliseconds-input').value = '';
        }
    }

    function updateIntervalFromMilliseconds() {
        const milliseconds = document.getElementById('milliseconds-input').value;
        if (milliseconds) {
            intervalTime = parseInt(milliseconds);
            document.getElementById('seconds-input').value = '';
        }
    }

    function startInterval() {
        if (isRunning) {
            showNotification("O script já está em execução");
            return;
        }

        const secondsInput = document.getElementById('seconds-input').value;
        const millisecondsInput = document.getElementById('milliseconds-input').value;

        if (!secondsInput && !millisecondsInput) {
            showNotification("defina um tempo em segundos ou milissegundos.");
            return;
        }

        if (isNaN(secondsInput) && isNaN(millisecondsInput)) {
            showNotification("Só números são aceitos nos textbox de tempo.");
            return;
        }

        if (secondsInput && millisecondsInput) {
            showNotification("Por favor escolha apenas um segundos ou milissegundos.");
            return;
        }

        isRunning = true;
        count = 0;
        const event = new KeyboardEvent('keydown', { key: 'ArrowRight', code: 'ArrowRight', keyCode: 39, which: 39, bubbles: true });

        intervalId = setInterval(() => {
            if (detectQuiz && (document.querySelector('[data-quiz]') || document.querySelector('#quiz'))) {
                if (autoContinueAfterQuiz) {
                    if (!quizCheckIntervalId) {
                        quizCheckIntervalId = setInterval(() => {
                            if (!document.querySelector('[data-quiz]') && !document.querySelector('#quiz')) {
                                clearInterval(quizCheckIntervalId);
                                quizCheckIntervalId = null;
                                showNotification("Quiz finalizado. Continuando a passagem de páginas.");
                                document.dispatchEvent(event);
                            }