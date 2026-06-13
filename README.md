<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Arquitetura de Computadores | Interface Retrô - Slide Interativo</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; user-select: none; }
    body { background: #008080; display: flex; justify-content: center; align-items: center; min-height: 100vh; font-family: 'Courier New', Courier, monospace; padding: 30px 20px; overflow: hidden; }
    
    /* JANELA PRINCIPAL RETRÔ */
    .retro-os { background-color: #c0c0c0; font-family: 'Courier New', Courier, monospace; width: 980px; max-width: 100%; margin: 0 auto; color: #000000; box-shadow: 8px 8px 0px rgba(0,0,0,0.3); border: 2px solid #ffffff; border-right-color: #404040; border-bottom-color: #404040; position: relative; z-index: 10; }
    .title-bar { background-color: #000080; color: #ffffff; padding: 6px 12px; display: flex; justify-content: space-between; align-items: center; font-weight: bold; font-size: 15px; font-family: 'Courier New', monospace; border-bottom: 1px solid #ffffff; }
    .title-icon { display: inline-block; width: 18px; height: 18px; background-color: #c0c0c0; margin-right: 8px; border: 1px solid #dfdfdf; border-right-color: #000; border-bottom-color: #000; text-align: center; line-height: 16px; font-size: 12px; color: #000; }
    .window-controls { display: flex; gap: 5px; }
    .win-btn { background-color: #c0c0c0; color: #000; border: 2px solid #fff; border-right-color: #000; border-bottom-color: #000; width: 24px; height: 24px; display: flex; justify-content: center; align-items: center; font-weight: bold; cursor: pointer; font-size: 14px; }
    .win-btn:active { border-top-color: #000; border-left-color: #000; border-right-color: #fff; border-bottom-color: #fff; transform: translate(1px, 1px); }
    
    .menu-bar { padding: 8px 12px 6px 12px; display: flex; gap: 15px; font-size: 14px; background-color: #c0c0c0; border-bottom: 2px solid #a0a0a0; flex-wrap: wrap; }
    .menu-item { cursor: pointer; padding: 2px 6px; border: 1px solid #c0c0c0; }
    .menu-item.active { font-weight: bold; background-color: #e0e0e0; border: 1px solid #a0a0a0; border-right-color: #f0f0f0; border-bottom-color: #f0f0f0; }
    .menu-item:hover { background-color: #d0d0d0; }
    
    .content-area { background-color: #ffffff; margin: 12px; border: 3px solid #000; border-top-color: #808080; border-left-color: #808080; display: flex; flex-wrap: wrap; min-height: 420px; }
    .text-column { flex: 1.2; padding: 24px 22px; font-size: 15px; line-height: 1.5; background-color: #fff; border-right: 1px solid #b0b0b0; }
    .text-column h3 { font-size: 22px; margin-bottom: 20px; border-left: 6px solid #000080; padding-left: 14px; }
    .text-column ul { padding-left: 28px; list-style: none; }
    .text-column li { margin-bottom: 14px; position: relative; }
    .text-column li::before { content: "►"; color: #000080; position: absolute; left: -22px; }
    .graphic-column { flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; gap: 20px; background: #fefefe; padding: 20px; min-width: 260px; }
    
    .interactive-panel { background: #d4d0c8; border: 3px solid #fff; border-right-color: #404040; border-bottom-color: #404040; width: 100%; text-align: center; padding: 16px 10px; cursor: pointer; transition: 0.05s linear; }
    .interactive-panel:active { border-top-color: #404040; border-left-color: #404040; border-right-color: #fff; border-bottom-color: #fff; transform: translate(1px, 1px); }
    .cpu-pixel { font-size: 56px; letter-spacing: 8px; filter: drop-shadow(2px 2px 0px #808080); }
    .counter-display { background: #000; color: #0f0; font-family: 'Courier New', monospace; font-size: 28px; font-weight: bold; padding: 10px; margin-top: 8px; border: 2px inset #808080; text-align: center; word-wrap: break-word; }
    
    .click-button { background: #c0c0c0; border: 3px solid #fff; border-right-color: #000; border-bottom-color: #000; padding: 10px; font-size: 16px; font-weight: bold; cursor: pointer; width: 100%; font-family: monospace; }
    .click-button:active { border-top-color: #000; border-left-color: #000; border-right-color: #fff; border-bottom-color: #fff; transform: translate(1px, 1px); }
    
    .timeline-interactive { display: flex; justify-content: space-between; gap: 8px; flex-wrap: wrap; margin-top: 10px; }
    .timeline-node { background: #c0c0c0; border: 2px solid #fff; border-right-color: #000; border-bottom-color: #000; padding: 8px 4px; font-size: 12px; font-weight: bold; cursor: pointer; flex: 1; text-align: center; }
    .timeline-node:active { transform: translate(1px, 1px); border-top-color: #000; border-left-color: #000; }
    
    .scrollbar-area { margin: 6px 12px 14px 12px; display: flex; height: 28px; gap: 4px; align-items: center; }
    .scroll-btn { width: 30px; background: #c0c0c0; border: 2px solid #fff; border-right-color: #000; border-bottom-color: #000; display: flex; justify-content: center; align-items: center; cursor: pointer; font-size: 16px; }
    .scroll-track { flex: 1; background: #b0b0b0; border-top: 2px solid #606060; border-left: 2px solid #606060; position: relative; height: 100%; }
    .scroll-thumb { width: 90px; height: 80%; background: #c0c0c0; border: 2px solid #fff; border-right-color: #404040; border-bottom-color: #404040; margin-left: 20px; cursor: pointer; }
    
    .retro-slide { position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: #ffffcc; border: 4px solid #000000; border-top-color: #ffffff; border-left-color: #ffffff; box-shadow: 8px 8px 0px rgba(0,0,0,0.3); font-family: 'Courier New', Courier, monospace; font-size: 14px; font-weight: bold; color: #000000; padding: 12px 20px; min-width: 240px; max-width: 340px; text-align: center; z-index: 9999; opacity: 0; transition: opacity 0.08s linear; pointer-events: none; letter-spacing: 0.5px; line-height: 1.4; word-wrap: break-word; }
    .retro-slide span { display: inline-block; background-color: #000080; color: white; padding: 2px 8px; margin-bottom: 6px; font-size: 11px; text-transform: uppercase; }
    .retro-slide p { margin: 6px 0 0 0; }
    .retro-slide::before { content: "📟"; position: absolute; left: -20px; top: -18px; font-size: 20px; opacity: 0.8; }

    /* ====== TELA AZUL DINÂMICA ====== */
    .bsod-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background-color: #0000AA;
      color: #FFFFFF;
      font-family: 'Courier New', Courier, monospace;
      z-index: 999999;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 40px;
      opacity: 0;
      visibility: hidden;
      cursor: pointer;
    }
    .bsod-overlay .os-label {
      background-color: #AAAAAA;
      color: #0000AA;
      padding: 2px 12px;
      font-weight: bold;
      font-size: 20px;
      margin-bottom: 40px;
    }
    .bsod-overlay p {
      margin: 10px 0;
      font-size: 20px;
      max-width: 800px;
      text-align: left;
      line-height: 1.4;
    }
    .bsod-overlay .blink {
      animation: blinker 1s linear infinite;
      margin-top: 40px;
      text-align: center;
    }
    @keyframes blinker {
      50% { opacity: 0; }
    }

    /* Ajuste para o painel de código ficar bem formatado */
    #codigoDisplay {
      white-space: pre-wrap; 
      text-align: left; 
      font-size: 16px; 
      padding: 15px; 
      line-height: 1.3;
      min-height: 130px;
      display: flex;
      align-items: center;
    }

    @media (max-width: 700px) { 
      .content-area { flex-direction: column; } 
      .text-column { border-right: none; } 
      .retro-slide { min-width: 200px; font-size: 12px; padding: 10px 16px; } 
      .menu-bar { justify-content: center; }
      .bsod-overlay p { font-size: 14px; }
      #codigoDisplay { font-size: 13px; min-height: 100px;}
    }
  </style>
</head>
<body>

<div class="retro-os">
  <div class="title-bar">
    <div><span class="title-icon">💾</span> Arquitetura de Computadores</div>
    <div class="window-controls">
      <div class="win-btn" onclick="mostrarSlide('Minimizar janela 🖥️ — puro estilo 95!')">_</div>
      <div class="win-btn" onclick="mostrarSlide('Tela cheia de conhecimento! Explore os menus interativos.')">□</div>
      <div class="win-btn" onclick="mostrarSlide('Sistema: Navegue pela evolução e clique nos elementos!')">✕</div>
    </div>
  </div>

  <div class="menu-bar">
    <span class="menu-item active" data-section="capa" onclick="trocarConteudo('capa')">Capa</span>
    <span class="menu-item" data-section="evolucao" onclick="trocarConteudo('evolucao')">Evolução</span>
    <span class="menu-item" data-section="neumann" onclick="trocarConteudo('neumann')">Von Neumann</span>
    <span class="menu-item" data-section="cpu" onclick="trocarConteudo('cpu')">CPU</span>
    <span class="menu-item" data-section="interrupcoes" onclick="trocarConteudo('interrupcoes')">Interrupções e Traps</span>
    <span class="menu-item" data-section="es" onclick="trocarConteudo('es')">E/S</span>
    <span class="menu-item" data-section="software" onclick="trocarConteudo('software')">Linguagens</span>
  </div>

  <div class="content-area" id="conteudoDinamico"></div>

  <div class="scrollbar-area">
    <div class="scroll-btn" onclick="rolarMensagem('left')">◄</div>
    <div class="scroll-track" id="scrollTrack">
      <div class="scroll-thumb" id="scrollThumb" draggable="false"></div>
    </div>
    <div class="scroll-btn" onclick="rolarMensagem('right')">►</div>
  </div>
</div>

<div id="retroSlide" class="retro-slide" style="opacity:0; visibility: hidden;"></div>

<div id="bsodOverlay" class="bsod-overlay" onclick="fecharBSOD()">
  <span class="os-label" id="bsodTitle">OC-DOS / SISTEMA</span>
  <p id="bsodDesc">Descrição do Erro.</p>
  <br>
  <div id="bsodFlow"></div>
  <br>
  <p class="blink">Pressione qualquer tecla ou clique para restaurar o contexto e continuar _</p>
</div>

<script>
  let slideTimeout = null;
  
  function mostrarSlide(mensagem) {
    const slide = document.getElementById('retroSlide');
    if (!slide) return;
    
    if (slideTimeout) clearTimeout(slideTimeout);
    
    slide.style.visibility = 'visible';
    slide.style.opacity = '1';
    slide.innerHTML = `<span>⏣ SISTEMA ⏣</span><p>${mensagem}</p>`;
    
    slideTimeout = setTimeout(() => {
      ocultarSlide();
    }, 7000); 
  }

  function ocultarSlide() {
    const slide = document.getElementById('retroSlide');
    if (!slide) return;
    slide.style.opacity = '0';
    setTimeout(() => {
      if (slide.style.opacity === '0') slide.style.visibility = 'hidden';
    }, 100);
  }
  
  // FUNCIONALIDADE DA TELA AZUL DINÂMICA
  window.mostrarBSOD = function(tipo) {
    const bsod = document.getElementById('bsodOverlay');
    const title = document.getElementById('bsodTitle');
    const desc = document.getElementById('bsodDesc');
    const flow = document.getElementById('bsodFlow');

    if (tipo === 'IRQ') {
      title.innerText = "OC-DOS / INTERRUPÇÃO (HARDWARE)";
      desc.innerText = "Uma interrupção de HARDWARE (IRQ) foi solicitada no endereço 0x0028:C0011E36.";
      flow.innerHTML = `
        <p>* O processador interrompeu o programa principal assincronamente.</p>
        <p>* Os valores dos registradores e o contador de programa (PC) foram salvos na Pilha.</p>
        <p>* O fluxo foi desviado para a Rotina de Serviço de Interrupção (ISR).</p>
      `;
    } else if (tipo === 'TRAP') {
      title.innerText = "OC-DOS / TRAP (SOFTWARE / SYSCALL)";
      desc.innerText = "Um evento SÍNCRONO (Trap) forçou a transição de modo no endereço 0x004A:F8A10002.";
      flow.innerHTML = `
        <p>* O programa solicitou um serviço privilegiado (Ex: Ler arquivo no disco).</p>
        <p>* Transição imediata: Modo de Usuário ➡️ Modo de Supervisor ativada.</p>
        <p>* O Sistema Operacional assume o controle com segurança máxima.</p>
      `;
    }

    bsod.style.visibility = 'visible';
    bsod.style.opacity = '1';
  };

  window.fecharBSOD = function() {
    const bsod = document.getElementById('bsodOverlay');
    if(bsod) {
      bsod.style.opacity = '0';
      setTimeout(() => { bsod.style.visibility = 'hidden'; }, 150);
    }
    
    const display = document.getElementById('cpuStatus');
    if(display) {
      display.style.color = "#0f0";
      display.innerText = "EXECUTANDO (Usuário)";
      mostrarSlide('🔙 Contexto Restaurado! O processador retomou a execução de onde parou na Memória Principal.');
    }
  };

  document.addEventListener('keydown', (e) => {
    const bsod = document.getElementById('bsodOverlay');
    if(bsod && bsod.style.visibility === 'visible') {
      window.fecharBSOD();
    }
  });
  
  let clickCountGlobal = 0;
  let geracaoAtual = "Transistores (1950)"; 
  let ciclosVon = 0;
  let counterCPUClock = 0;
  let modoESIdx = 0;
  let codeStep = 0;
  
  function trocarConteudo(secao) {
    ocultarSlide(); 

    const menuItems = document.querySelectorAll('.menu-item');
    menuItems.forEach(item => {
      item.classList.remove('active');
      if(item.getAttribute('data-section') === secao) item.classList.add('active');
    });
    const container = document.getElementById('conteudoDinamico');
    if(!container) return;
    
    // CAPA
    if(secao === 'capa') {
      container.innerHTML = `
        <div class="text-column">
          <h3>💻 ORGANIZAÇÃO DE COMPUTADORES</h3>
          <p><b>Trabalho Acadêmico</b></p>
          <br>
          <p>Explore a evolução arquitetônica, o modelo de Von Neumann, sistemas de E/S, interrupções e o fluxo de software conosco!</p>
          <br>
          <div style="background:#000080; color:#fff; padding:12px; border:3px solid #c0c0c0; border-right-color:#000; border-bottom-color:#000; box-shadow: inset 2px 2px 0px #404040;">
            <p style="margin-bottom: 8px; text-transform: uppercase; border-bottom: 1px solid #fff; padding-bottom: 4px;"><b>👨‍💻 Equipe de Desenvolvimento:</b></p>
            <ul style="list-style: none; padding-left: 5px; font-weight: bold; font-size: 14px;">
              <li style="margin-bottom: 6px;">> Miguel de Souza</li>
              <li style="margin-bottom: 6px;">> Pedro H. Stulpen</li>
              <li style="margin-bottom: 6px;">> Raphael Gonçalves</li>
            </ul>
          </div>
          <div style="margin-top:20px; background:#ffffcc; padding:10px; border:2px solid #000; border-left:6px solid #000080; font-size: 13px;">
            💡 <b>DICA DO SISTEMA:</b> Utilizaremos uma interface interativa para trazer mais dinamismo ao projeto.
          </div>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="painelCapa">
            <div class="cpu-pixel">💾 🖥️ ⚡</div>
            <div style="font-weight:bold; margin:8px 0">INICIAR SISTEMA</div>
            <div id="contadorCapa" class="counter-display">0</div>
            <div style="font-size:12px; margin-top:6px;">Eventos gerados</div>
          </div>
          <button class="click-button" id="resetCapaBtn">🔄 Resetar eventos</button>
          <div class="timeline-interactive" id="linhaTempoCapa">
            <div class="timeline-node" data-era="eniac">1945 ENIAC</div>
            <div class="timeline-node" data-era="transistor">1954 TRANS.</div>
            <div class="timeline-node" data-era="micro">1971 4004</div>
            <div class="timeline-node" data-era="pentium">1993 PENTIUM</div>
          </div>
        </div>
      `;
      
      document.querySelectorAll('#linhaTempoCapa .timeline-node').forEach(el => {
        el.addEventListener('click', (e) => {
          e.stopPropagation();
          const era = el.getAttribute('data-era');
          if(era === 'eniac') mostrarSlide('📡 ENIAC (1945): 18.000 válvulas, 30 toneladas! Primeiro computador eletrônico de grande escala.');
          else if(era === 'transistor') mostrarSlide('⚛️ 1954: Transistores revolucionam o consumo e confiabilidade. Início da 2ª geração.');
          else if(era === 'micro') mostrarSlide('🧠 1971: Intel 4004, primeiro microprocessador comercial. 2300 transistores!');
          else if(era === 'pentium') mostrarSlide('🚀 1993: Intel Pentium, 3.1 milhões de transistores, arquitetura superscalar.');
        });
      });
      
      if(!window.contadorGlobalCapa) window.contadorGlobalCapa = 0;
      document.getElementById('contadorCapa').innerText = window.contadorGlobalCapa;
      
      document.getElementById('painelCapa').onclick = () => {
        let mensagensEra = [
          "SISTEMA INICIADO: Bem-vindo ao Trabalho de OC1!",
          "Verificando integridade da memória principal...",
          "Carregando barramentos de Entrada e Saída...",
          "Interrupções ativadas. Sistema pronto para uso!",
          "🚀 Explore os menus acima para começar a apresentação."
        ];
        if (window.contadorGlobalCapa < mensagensEra.length) {
          window.contadorGlobalCapa++;
          document.getElementById('contadorCapa').innerText = window.contadorGlobalCapa;
          mostrarSlide(`💻 ${mensagensEra[window.contadorGlobalCapa - 1]}`);
        } else {
          ocultarSlide();
        }
      };
      
      document.getElementById('resetCapaBtn').onclick = () => {
        window.contadorGlobalCapa = 0;
        document.getElementById('contadorCapa').innerText = "0";
        mostrarSlide('🔄 Contador zerado! Recomece a jornada da computação.');
      };
    }
    // EVOLUÇÃO
    else if(secao === 'evolucao') {
      container.innerHTML = `
        <div class="text-column">
          <h3>🔧 EVOLUÇÃO - LINHA DO TEMPO</h3>
          <ul>
            <li><b>1945:</b> Válvulas, ENIAC</li>
            <li><b>1950s:</b> Transistores</li>https://www.onlinegdb.com/online_html_compiler#tab-stdin
            <li><b>1960s:</b> Circuitos Integrados</li>
            <li><b>1971:</b> Microprocessador</li>
          </ul>
          <p id="mensagemEvolucao" style="background:#00008020; padding:6px; margin-top:16px;">⚡ Clique no chip ao lado para simular avanços!</p>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="painelEvolucao">
            <div style="font-size:42px;">🔘💿</div>
            <div style="font-weight:bold; margin:6px 0">GERAÇÃO ATUAL:</div>
            <div id="geracaoDisplay" class="counter-display" style="font-size:20px;">Transistores (1950)</div>
            <div style="margin-top:10px;">Clique para avançar a era</div>
          </div>
          <button class="click-button" id="resetEvolucaoBtn">🔄 Reset Evolução</button>
          <div class="timeline-interactive" id="timelineEvol">
            <div class="timeline-node" data-tipo="valvulas">📡 Válvulas</div>
            <div class="timeline-node" data-tipo="transistores">⚛️ Transistores</div>
            <div class="timeline-node" data-tipo="circuitos">🔌 C. Integrados</div>
            <div class="timeline-node" data-tipo="micro">🧠 Microprocessador</div>
          </div>
        </div>
      `;
      document.getElementById('geracaoDisplay').innerText = geracaoAtual;
      
      document.getElementById('painelEvolucao').onclick = () => {
        const eras = ["Válvulas (1945)", "Transistores (1950)", "Circuitos Integrados (1960)", "Microprocessador (1971)", "Multicore (2000+)", "Arquitetura Quântica (Futuro)"];
        let idx = eras.indexOf(geracaoAtual);
        if(idx === -1) idx = 1;
        
        let novaIdx = idx + 1;
        if (novaIdx < eras.length) {
          geracaoAtual = eras[novaIdx];
          document.getElementById('geracaoDisplay').innerText = geracaoAtual;
          mostrarSlide(`✨ EVOLUÇÃO: ${geracaoAtual} ✨ Avanço tecnológico marcante!`);
        } else {
          ocultarSlide();
        }
      };
      
      document.getElementById('resetEvolucaoBtn').onclick = () => {
        geracaoAtual = "Transistores (1950)";
        document.getElementById('geracaoDisplay').innerText = geracaoAtual;
        mostrarSlide('🔄 Evolução reiniciada para a era dos Transistores.');
      };
      
      document.querySelectorAll('#timelineEvol .timeline-node').forEach(el => {
        el.addEventListener('click', (e) => {
          e.stopPropagation();
          const tipo = el.getAttribute('data-tipo');
          if(tipo === 'valvulas') mostrarSlide('🔴 VÁLVULAS: ENIAC usava válvulas com alta taxa de falhas e consumo extremo, 1945.');
          else if(tipo === 'transistores') mostrarSlide('🔘 TRANSISTORES: Bell Labs 1947, menor consumo e mais confiáveis, revolução.');
          else if(tipo === 'circuitos') mostrarSlide('📟 CI: Múltiplos transistores em um chip (LSI/VLSI). Miniaturização!');
          else if(tipo === 'micro') mostrarSlide('🧠 MICROPROCESSADOR: Arquiteturas contemporâneas estabelecidas.');
        });
      });
    }
    // VON NEUMANN
    else if(secao === 'neumann') {
      container.innerHTML = `
        <div class="text-column">
          <h3>🏛️ Arquitetura Von Neumann</h3>
          <p>Modelo de programa armazenado. Componentes: Memória, CPU (ULA + UC) e Barramentos. <b>Clique no chip interativo!</b></p>
          <ul>
            <li>As instruções devem ser buscadas diretamente na MP.</li>
            <li>Codificação binária de instruções.</li>
          </ul>
          <div id="feedbackVon" style="margin-top:14px;">🔍 Pressione o botão gráfico</div>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="vonPanel">
            <div style="font-size:44px;">🏛️🔁💾</div>
            <div style="margin:8px 0">CICLO DE INSTRUÇÃO</div>
            <div id="vonCount" class="counter-display" style="font-size:20px;">0 ciclos</div>
          </div>
          <button class="click-button" id="resetVonBtn">Redefinir ciclos</button>
        </div>
      `;
      if(!window.ciclosVon) window.ciclosVon = 0;
      document.getElementById('vonCount').innerText = window.ciclosVon + " ciclos";
      document.getElementById('vonPanel').onclick = () => {
        window.ciclosVon++;
        document.getElementById('vonCount').innerText = window.ciclosVon + " ciclos";
        mostrarSlide(`🔄 Ciclo Von Neumann #${window.ciclosVon}: Ler CI → REM → Acesso à Memória → Decodifica no RI → Executa!`);
      };
      document.getElementById('resetVonBtn').onclick = () => {
        window.ciclosVon = 0;
        document.getElementById('vonCount').innerText = "0 ciclos";
        mostrarSlide('Contador de ciclos zerado. Reexecute o barramento!');
      };
    }
    // CPU
    else if(secao === 'cpu') {
      container.innerHTML = `
        <div class="text-column">
          <h3>⚙️ CPU - UNIDADE CENTRAL</h3>
          <p>A CPU atua como o núcleo operacional. É dividida em ULA (operações lógicas e matemáticas) e UC (emissão de sinais de controle).</p>
          <ul>
            <li>Desempenho depende da arquitetura interna, não só do clock.</li>
          </ul>
          <div id="mensagemCPU" style="background:#c0c0c040; padding:5px;">Clique no chip ou no botão turbo!</div>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="cpuPanel">
            <div style="font-size:48px;">🧮⚡</div>
            <div style="font-weight:bold;">CLOCK ANALÓGICO</div>
            <div id="clockCounter" class="counter-display">0 MHz</div>
          </div>
          <button class="click-button" id="turboBtn">🚀 TURBO MODE</button>
        </div>
      `;
      if(!window.counterCPUClock) window.counterCPUClock = 0;
      function atualizarClockDisplay() {
        const disp = document.getElementById('clockCounter');
        if(disp) {
          let freq = window.counterCPUClock;
          if(freq >= 1000) disp.innerText = (freq/1000).toFixed(1) + " GHz";
          else disp.innerText = freq + " MHz";
        }
      }
      atualizarClockDisplay();
      document.getElementById('cpuPanel').onclick = () => {
        window.counterCPUClock += 10;
        atualizarClockDisplay();
        mostrarSlide(`⚡ Oscilador de quartzo (Clock) simulado aumentado! Frequência: ${window.counterCPUClock} MHz.`);
      };
      document.getElementById('turboBtn').onclick = () => {
        window.counterCPUClock += 50;
        atualizarClockDisplay();
        mostrarSlide(`🚀 MODO TURBO! Lembre-se: O desempenho real resulta de fatores arquitetônicos, não só do clock.`);
      };
    }
    // INTERRUPÇÕES E TRAPS (AMBOS COM BSOD AGORA)
    else if(secao === 'interrupcoes') {
      container.innerHTML = `
        <div class="text-column">
          <h3> INTERRUPÇÕES E TRAPS</h3>
          <p>Mecanismos fundamentais que alteram o fluxo natural da CPU.</p>
          <ul>
            <li><b>Interrupção (IRQ):</b> Assíncrono. Hardware (ex: teclado) avisa a CPU. Impede que a CPU precise ficar checando (polling).</li>
            <li><b>Trap (Syscall):</b> Síncrono. Software pede acesso ao Kernel intencionalmente.</li>
            <li><b>Privilégios:</b> Mudança de Estado de Usuário (restrito) para Supervisor (total).</li>
          </ul>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="intPanel">
            <div style="font-size:42px;">⚠️</div>
            <div style="font-weight:bold; margin:6px 0">STATUS DA CPU</div>
            <div id="cpuStatus" class="counter-display" style="color:#0f0; font-size: 20px;">EXECUTANDO (Usuário)</div>
          </div>
          <button class="click-button" id="btnInterrupt" style="color:red; margin-bottom: 8px;">💥 Gerar Interrupção (Hardware)</button>
          <button class="click-button" id="btnTrap" style="color:#0000AA;">⚙️ Gerar Trap (Software)</button>
        </div>
      `;
      
      document.getElementById('btnInterrupt').onclick = () => {
        document.getElementById('cpuStatus').style.color = "#ff3333";
        document.getElementById('cpuStatus').innerText = "ESPERANDO ISR...";
        mostrarBSOD('IRQ');
      };
      
      document.getElementById('btnTrap').onclick = () => {
        document.getElementById('cpuStatus').style.color = "#ffff33";
        document.getElementById('cpuStatus').innerText = "MODO SUPERVISOR";
        mostrarBSOD('TRAP');
      };
    }
    // ENTRADA E SAÍDA (E/S)
    else if(secao === 'es') {
      container.innerHTML = `
        <div class="text-column">
          <h3>🖨️ SUBSISTEMA DE E/S</h3>
          <p>Dispositivos de E/S estabelecem comunicação com o mundo exterior. Barreira crítica: são exponencialmente mais lentos que a CPU e MP.</p>
          <ul>
            <li><b>Transmissão:</b> Paralela (múltiplos bits, curta distância) vs Serial (um bit sequencial, longa distância).</li>
            <li><b>Sincronia:</b> Síncrona (clock compartilhado) vs Assíncrona (start/stop bits).</li>
            <li><b>Discos Magnéticos:</b> Tempo de Acesso = Seek + Latência + Transferência.</li>
          </ul>
        </div>
        <div class="graphic-column">
          <div class="interactive-panel" id="esPanel">
            <div style="font-size:42px;">📡</div>
            <div style="font-weight:bold; margin:6px 0">MODO DO CANAL</div>
            <div id="modoES" class="counter-display" style="font-size:22px;">SIMPLEX ➡️</div>
          </div>
          <button class="click-button" id="btnTrocarModo">Alterar Modo (Direção)</button>
        </div>
      `;
      const modos = [
        { label: "SIMPLEX ➡️", desc: "Transmissão unidirecional estrita (ex: TV)." },
        { label: "HALF-DUPLEX ↔️", desc: "Bidirecional alternado. Não simultâneo (ex: Walkie-Talkie)." },
        { label: "FULL-DUPLEX 🔁", desc: "Bidirecional simultâneo. Maximiza banda." }
      ];
      document.getElementById('btnTrocarModo').onclick = () => {
        modoESIdx = (modoESIdx + 1) % modos.length;
        document.getElementById('modoES').innerText = modos[modoESIdx].label;
        mostrarSlide(`🔄 MODO ALTERADO: ${modos[modoESIdx].desc}`);
      };
    }
    // SOFTWARE E LINGUAGEM (ATUALIZADO E DETALHADO)
    else if(secao === 'software') {
      container.innerHTML = `
        <div class="text-column" style="flex: 1;">
          <h3>💻 EXECUÇÃO E LINGUAGENS</h3>
          <p>Para a CPU processar dados, a lógica humana precisa ser traduzida sucessivamente até virar pulsos elétricos.</p>
          <ul>
            <li><b>Alto Nível:</b> Foco no programador. Sintaxe próxima da linguagem humana (Python, Java).</li>
            <li><b>Baixo Nível:</b> Foco na máquina. Gerenciamento explícito de registradores (C, Assembly).</li>
            <li><b>Máquina:</b> Binário executável nativo. A única linguagem que o hardware de fato "entende".</li>
          </ul>
        </div>
        <div class="graphic-column" style="flex: 1.5; min-width: 320px;">
          <div class="interactive-panel" id="softPanel" style="padding: 10px; cursor: default;">
            <div id="linguagemNome" style="font-weight:bold; margin-bottom:10px; font-size: 18px; color:#000080;">🐍 PYTHON (Alto Nível)</div>
            <div id="codigoDisplay" class="counter-display">print("Olá, Mundo!")</div>
          </div>
          <button class="click-button" id="btnCompilar">Descer Nível de Abstração ⬇️</button>
        </div>
      `;
      
      const steps = [
        { 
          lang: " PYTHON (Alto Nível)", 
          code: "print(\"Olá, Mundo!\")", 
          desc: "Alta Abstração: Código limpo, interpretado em tempo de execução. Esconde a complexidade da memória." 
        },
        { 
          lang: "LINGUAGEM C (Médio/Alto Nível)", 
          code: "#include <stdio.h>\n\nint main() {\n  printf(\"Olá, Mundo!\\n\");\n  return 0;\n}", 
          desc: "Linguagem Compilada: Permite controle direto do hardware e ponteiros, muito usada em Sistemas Operacionais." 
        },
        { 
          lang: "🧮 ASSEMBLY x86 (Baixo Nível)", 
          code: "MOV AH, 09H\nLEA DX, msg\nINT 21H", 
          desc: "Linguagem de Montagem: Mnemônicos que mapeiam quase 1:1 com as instruções reais do processador." 
        },
        { 
          lang: "MÁQUINA (Binário Puro)", 
          code: "10110100 00001001\n10001101 00010110\n11001101 00100001", 
          desc: "O que a CPU vê! Sequências binárias interpretadas pelos circuitos da Unidade de Controle e ULA." 
        }
      ];
      
      // Garante que ao entrar na aba o passo comece correto
      codeStep = 0; 
      
      document.getElementById('btnCompilar').onclick = () => {
        codeStep = (codeStep + 1) % steps.length;
        document.getElementById('linguagemNome').innerText = steps[codeStep].lang;
        document.getElementById('codigoDisplay').innerText = steps[codeStep].code;
        mostrarSlide(`🔍 ${steps[codeStep].desc}`);
      };
    }
  }
  
  function rolarMensagem(dir) {
    const thumb = document.getElementById('scrollThumb');
    if(!thumb) return;
    let left = parseInt(thumb.style.marginLeft) || 20;
    const track = document.querySelector('.scroll-track');
    const maxLeft = (track ? track.clientWidth - thumb.clientWidth - 6 : 150);
    if(dir === 'left') left = Math.max(4, left - 40);
    else left = Math.min(maxLeft, left + 40);
    thumb.style.marginLeft = left + 'px';
    const mensagensRoll = [
      "📀 1945: Nascimento do ENIAC - Válvulas eletrônicas",
      "⚠️ Interrupções protegem contra monopólio de CPU",
      "📡 E/S: Tempo de Acesso ao Disco = Seek + Latência + Transfer",
      "🧠 Von Neumann: Acesso direto à Memória Principal",
      "💻 Compilação (rápida/estática) vs Interpretação (lenta/dinâmica)"
    ];
    let idx = Math.floor(Math.random() * mensagensRoll.length);
    mostrarSlide(`⏪croll: ${mensagensRoll[idx]}`);
  }
  
  function initDragThumb() {
    const thumb = document.getElementById('scrollThumb');
    const track = document.getElementById('scrollTrack');
    if(!thumb || !track) return;
    let isDragging = false;
    thumb.addEventListener('mousedown', (e) => { isDragging = true; e.preventDefault(); });
    document.addEventListener('mousemove', (e) => {
      if(!isDragging) return;
      const rect = track.getBoundingClientRect();
      let newLeft = e.clientX - rect.left - thumb.clientWidth/2;
      newLeft = Math.min(Math.max(newLeft, 2), rect.width - thumb.clientWidth - 4);
      thumb.style.marginLeft = newLeft + 'px';
    });
    document.addEventListener('mouseup', () => { isDragging = false; });
  }
  
  window.addEventListener('DOMContentLoaded', () => {
    trocarConteudo('capa');
    initDragThumb();
    window.contadorGlobalCapa = 0;
    window.ciclosVon = 0;
    window.counterCPUClock = 0;
  });
  
  window.trocarConteudo = trocarConteudo;
  window.mostrarSlide = mostrarSlide;
  window.rolarMensagem = rolarMensagem;
</script>
</body>
</html>
