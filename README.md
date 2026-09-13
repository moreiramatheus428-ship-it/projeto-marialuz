<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Instituto Maria Luz</title>
  <style>
    :root{
      --navy:#16325C; --paper:#F6F4EE; --ink:#21221D; --muted:#6B6A63; --line:#DCD8CC;
      --hand1:#2E86AB; --hand2:#E4572E; --hand3:#F2B134; --hand4:#7B4CA0; --hand5:#3B8C6E;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    }
    html,body{height:100%;margin:0;background:var(--paper);color:var(--ink)}
    .max-w-5xl{max-width:1000px;margin:0 auto}
    header{background:var(--navy);color:white}
    .header-inner{display:flex;flex-wrap:wrap;align-items:center;justify-content:space-between;padding:16px 20px;gap:12px}
    .brand{display:flex;align-items:center;gap:12px;border:none;background:none;color:inherit;cursor:pointer}
    .brand .logo{width:44px;height:44px;border-radius:12px;display:flex;align-items:center;justify-content:center;background:white;color:var(--navy);font-size:18px;overflow:hidden}
    nav.buttons{display:flex;flex-wrap:wrap;gap:8px;align-items:center}
    button.link{background:transparent;border-radius:6px;padding:8px 10px;border:none;color:inherit;cursor:pointer}
    button.link.active{background:rgba(255,255,255,0.12)}
    .subtitle{color:#B9C6DA;font-size:12px}
    main{min-height:60vh}
    .section {padding:28px 20px}
    .card{background:white;border:1px solid var(--line);border-radius:10px;padding:16px}
    input,select,textarea{padding:8px;border:1px solid var(--line);border-radius:6px;font-size:14px;width:100%;box-sizing:border-box}
    .grid{display:grid;gap:12px}
    .grid.cols-3{grid-template-columns:repeat(3,1fr)}
    .course-card{display:flex;flex-direction:column;border-radius:10px;overflow:hidden;text-align:left;cursor:pointer;border:1px solid var(--line);background:white}
    .course-card .topbar{height:6px}
    .course-card .body{padding:14px;display:flex;flex-direction:column;gap:10px;height:160px;box-sizing:border-box}
    .small{font-size:12px;color:var(--muted)}
    .muted{color:var(--muted)}
    .btn{padding:10px 14px;border-radius:8px;border:none;cursor:pointer}
    .btn.primary{background:var(--navy);color:white}
    .btn.ghost{background:transparent;border:1px solid var(--line)}
    .footer{background:var(--navy);color:#B9C6DA;padding:18px;text-align:center}
    .hand-stripe{display:flex;align-items:flex-end;gap:6px;height:24px}
    .hand-stripe div{border-radius:999px}
    .lesson-row{display:flex;align-items:center;gap:12px;padding:12px;border-bottom:1px solid var(--line);background:white;cursor:pointer}
    .badge{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:600}
    table{width:100%;border-collapse:collapse}
    th,td{padding:8px;text-align:left;border-bottom:1px solid var(--line)}
    .center{text-align:center}
    .progress-bar{background:var(--line);height:8px;border-radius:999px;overflow:hidden}
    .progress-bar > div{height:100%;background:var(--navy)}
    .text-success{color:#3B8C6E}
    .text-danger{color:#C0392B}
    .hidden{display:none}
    @media (max-width:800px){
      .grid.cols-3{grid-template-columns:1fr}
      .header-inner{flex-direction:column;align-items:flex-start}
    }
  </style>
</head>
<body>
  <header>
    <div class="max-w-5xl header-inner">
      <button class="brand" id="home-btn" title="Início">
        <div class="logo" id="logo-img"></div>
        <div>
          <div style="font-weight:800;font-size:18px">Instituto Maria Luz</div>
          <div class="subtitle">#EducandoParaUmFuturoMelhor</div>
        </div>
      </button>
      <nav class="buttons" id="main-nav"></nav>
    </div>
    <div class="max-w-5xl" id="student-info" style="padding:8px 20px;display:none;color:#B9C6DA;font-size:13px"></div>
  </header>

  <main id="app"></main>

  <footer class="footer">Instituto Maria Luz — #EducandoParaUmFuturoMelhor</footer>

<script>
/* ---------------- constantes & dados ---------------- */
const NAVY = "#16325C";
const PAPER = "#F6F4EE";
const INK = "#21221D";
const MUTED = "#6B6A63";
const LINE = "#DCD8CC";
const HAND_COLORS = ["#2E86AB", "#E4572E", "#F2B134", "#7B4CA0", "#3B8C6E"];

/* Logo embutido via SVG Data URL */
const LOGO_SRC = 'data:image/svg+xml;utf8,' + encodeURIComponent(`
  <svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 200 200">
    <rect width="100%" height="100%" rx="20" fill="#16325C"/>
    <circle cx="100" cy="80" r="45" fill="#ffffff"/>
    <text x="100" y="93" font-family="Arial, sans-serif" font-size="32" fill="#16325C" text-anchor="middle" font-weight="800">IML</text>
    <rect x="30" y="145" width="140" height="16" rx="8" fill="#F2B134"/>
  </svg>
`);

/* Estrutura completa de Cursos */
const COURSES = [
  { 
    id: "ingles", 
    name: "Inglês Iniciante", 
    tagline: "Primeiros passos para se comunicar em inglês no dia a dia.", 
    color: "#2E86AB", 
    soft: "#EAF4F8", 
    speechLang: "en-US", 
    emoji: "🌐",
    lessons: [
      { 
        title: "Cumprimentos e Apresentações", 
        intro: "Vamos aprender a cumprimentar pessoas e se apresentar em inglês — o primeiro passo em qualquer conversa.", 
        vocab: [
          {term:"Hello",translation:"Olá"},
          {term:"Good morning",translation:"Bom dia"},
          {term:"Good afternoon",translation:"Boa tarde"},
          {term:"My name is Maria",translation:"Meu nome é Maria"},
          {term:"What is your name?",translation:"Qual é o seu nome?"},
          {term:"Nice to meet you",translation:"Prazer em conhecer você"},
          {term:"How are you?",translation:"Como você está?"},
          {term:"I am fine, thank you",translation:"Estou bem, obrigado(a)"}
        ],
        quiz: [
          { q: "Qual frase significa 'Bom dia' em inglês?", options: ["Good night", "Good morning", "Good evening", "Goodbye"], correct: 1 },
          { q: "'What is your name?' é usado para:", options: ["Perguntar a idade", "Perguntar o nome", "Perguntar o endereço", "Se despedir"], correct: 1 },
          { q: "A tradução de 'Nice to meet you' é:", options: ["Até mais tarde", "Prazer em conhecer você", "Como vai?", "Muito obrigado"], correct: 1 },
          { q: "Complete: '___, my name is Maria.'", options: ["Hello", "Blue", "Ten", "House"], correct: 0 }
        ]
      },
      { 
        title: "Números e Cores", 
        intro: "Nesta aula você vai aprender a contar de um a cinco e a reconhecer algumas cores básicas em inglês.", 
        vocab: [
          {term:"One",translation:"Um"},
          {term:"Two",translation:"Dois"},
          {term:"Three",translation:"Três"},
          {term:"Four",translation:"Quatro"},
          {term:"Five",translation:"Cinco"},
          {term:"Red",translation:"Vermelho"},
          {term:"Blue",translation:"Azul"},
          {term:"Green",translation:"Verde"},
          {term:"Yellow",translation:"Amarelo"}
        ],
        quiz: [
          { q: "Como se diz 'três' em inglês?", options: ["Two", "Three", "Four", "Five"], correct: 1 },
          { q: "'Blue' significa:", options: ["Verde", "Amarelo", "Azul", "Vermelho"], correct: 2 },
          { q: "'Red' é a cor:", options: ["Vermelho", "Azul", "Verde", "Amarelo"], correct: 0 },
          { q: "'Five' corresponde ao número:", options: ["3", "4", "5", "6"], correct: 2 }
        ]
      },
      { 
        title: "A Família", 
        intro: "Aprenda o nome dos principais membros da família em inglês.", 
        vocab: [
          {term:"Mother",translation:"Mãe"},
          {term:"Father",translation:"Pai"},
          {term:"Sister",translation:"Irmã"},
          {term:"Brother",translation:"Irmão"},
          {term:"Grandmother",translation:"Avó"},
          {term:"Grandfather",translation:"Avô"}
        ],
        quiz: [
          { q: "'Mother' significa:", options: ["Pai", "Irmã", "Mãe", "Avó"], correct: 2 },
          { q: "A palavra para 'irmão' é:", options: ["Sister", "Brother", "Father", "Grandfather"], correct: 1 },
          { q: "'Grandfather' é:", options: ["Avô", "Avó", "Pai", "Tio"], correct: 0 },
          { q: "'Sister' significa:", options: ["Irmão", "Irmã", "Mãe", "Filha"], correct: 1 }
        ]
      },
      { 
        title: "Verbo To Be", 
        intro: "O verbo 'to be' (ser/estar) é um dos mais importantes do inglês. Veja como usá-lo com cada pessoa.", 
        vocab: [
          {term:"I am",translation:"Eu sou / estou"},
          {term:"You are",translation:"Você é / está"},
          {term:"He is / She is",translation:"Ele é / Ela é (está)"},
          {term:"We are",translation:"Nós somos / estamos"},
          {term:"They are",translation:"Eles são / estão"}
        ],
        quiz: [
          { q: "Complete: 'I ___ a student.'", options: ["is", "are", "am", "be"], correct: 2 },
          { q: "Complete: 'She ___ happy.'", options: ["am", "is", "are", "be"], correct: 1 },
          { q: "Complete: 'They ___ friends.'", options: ["is", "am", "are", "be"], correct: 2 },
          { q: "A forma correta para 'nós' é:", options: ["I am", "You are", "We are", "He is"], correct: 2 }
        ]
      },
      { 
        title: "Rotina Diária", 
        intro: "Vamos aprender palavras usadas para falar sobre a rotina do dia a dia.", 
        vocab: [
          {term:"Wake up",translation:"Acordar"},
          {term:"Have breakfast",translation:"Tomar café da manhã"},
          {term:"Go to school",translation:"Ir à escola"},
          {term:"Study",translation:"Estudar"},
          {term:"Sleep",translation:"Dormir"}
        ],
        quiz: [
          { q: "'Wake up' significa:", options: ["Dormir", "Acordar", "Estudar", "Comer"], correct: 1 },
          { q: "'Have breakfast' é usado para dizer:", options: ["Tomar café da manhã", "Almoçar", "Jantar", "Dormir"], correct: 0 },
          { q: "'Study' significa:", options: ["Trabalhar", "Estudar", "Descansar", "Viajar"], correct: 1 },
          { q: "'Go to school' significa:", options: ["Ir à escola", "Ir ao mercado", "Ir ao médico", "Ir para casa"], correct: 0 }
        ]
      }
    ]
  },
  { 
    id: "espanhol", 
    name: "Espanhol Iniciante", 
    tagline: "Los primeros pasos para hablar español todos los días.", 
    color: "#E4572E", 
    soft: "#FDECE6", 
    speechLang: "es-ES", 
    emoji: "🗣️",
    lessons: [
      { 
        title: "Saludos y Presentaciones", 
        intro: "Vamos aprender a cumprimentar e se apresentar em espanhol.", 
        vocab: [
          {term:"Hola",translation:"Olá"},
          {term:"Buenos días",translation:"Bom dia"},
          {term:"Buenas tardes",translation:"Boa tarde"},
          {term:"Me llamo Ana",translation:"Meu nome é Ana"},
          {term:"¿Cómo te llamas?",translation:"Qual é o seu nome?"},
          {term:"Mucho gusto",translation:"Muito prazer"},
          {term:"¿Cómo estás?",translation:"Como você está?"},
          {term:"Estoy bien, gracias",translation:"Estou bem, obrigado(a)"}
        ],
        quiz: [
          { q: "'Buenos días' significa:", options: ["Boa noite", "Bom dia", "Boa tarde", "Adeus"], correct: 1 },
          { q: "'¿Cómo te llamas?' pergunta:", options: ["A idade", "O nome", "O endereço", "A profissão"], correct: 1 },
          { q: "'Mucho gusto' significa:", options: ["Até logo", "Muito prazer", "Como vai", "Por favor"], correct: 1 },
          { q: "Complete: '___, me llamo Ana.'", options: ["Hola", "Azul", "Diez", "Casa"], correct: 0 }
        ]
      },
      { 
        title: "Números y Colores", 
        intro: "Aprenda a contar de um a cinco e reconhecer cores básicas em espanhol.", 
        vocab: [
          {term:"Uno",translation:"Um"},
          {term:"Dos",translation:"Dois"},
          {term:"Tres",translation:"Três"},
          {term:"Cuatro",translation:"Quatro"},
          {term:"Cinco",translation:"Cinco"},
          {term:"Rojo",translation:"Vermelho"},
          {term:"Azul",translation:"Azul"},
          {term:"Verde",translation:"Verde"},
          {term:"Amarillo",translation:"Amarelo"}
        ],
        quiz: [
          { q: "Como se diz 'três' em espanhol?", options: ["Dos", "Tres", "Cuatro", "Cinco"], correct: 1 },
          { q: "'Azul' em espanhol se escreve:", options: ["Azul", "Rojo", "Verde", "Amarillo"], correct: 0 },
          { q: "'Rojo' significa:", options: ["Verde", "Amarelo", "Vermelho", "Azul"], correct: 2 },
          { q: "'Cinco' corresponde ao número:", options: ["3", "4", "5", "6"], correct: 2 }
        ]
      },
      { 
        title: "La Familia", 
        intro: "Aprenda o nome dos principais membros da família em espanhol.", 
        vocab: [
          {term:"Madre",translation:"Mãe"},
          {term:"Padre",translation:"Pai"},
          {term:"Hermana",translation:"Irmã"},
          {term:"Hermano",translation:"Irmão"},
          {term:"Abuela",translation:"Avó"},
          {term:"Abuelo",translation:"Avô"}
        ],
        quiz: [
          { q: "'Madre' significa:", options: ["Pai", "Irmã", "Mãe", "Avó"], correct: 2 },
          { q: "A palavra para 'irmão' é:", options: ["Hermana", "Hermano", "Padre", "Abuelo"], correct: 1 },
          { q: "'Abuelo' é:", options: ["Avô", "Avó", "Pai", "Tio"], correct: 0 },
          { q: "'Hermana' significa:", options: ["Irmão", "Irmã", "Mãe", "Filha"], correct: 1 }
        ]
      },
      { 
        title: "Ser y Estar (básico)", 
        intro: "Veja como usar os verbos ser e estar em frases simples.", 
        vocab: [
          {term:"Yo soy",translation:"Eu sou"},
          {term:"Tú eres",translation:"Você é"},
          {term:"Él es / Ella es",translation:"Ele é / Ela é"},
          {term:"Yo estoy feliz",translation:"Eu estou feliz"},
          {term:"Nosotros estamos aquí",translation:"Nós estamos aqui"}
        ],
        quiz: [
          { q: "A tradução de 'Yo soy' é:", options: ["Eu estou", "Eu sou", "Ele é", "Nós somos"], correct: 1 },
          { q: "Complete: 'Tú ___ mi amigo.'", options: ["soy", "eres", "es", "somos"], correct: 1 },
          { q: "'Yo estoy feliz' significa:", options: ["Eu sou feliz", "Eu estou feliz", "Ela é feliz", "Eles estão bem"], correct: 1 },
          { q: "Como se diz 'Nós estamos aqui'?", options: ["Yo soy aquí", "Nosotros estamos aquí", "Tú eres aquí", "Él es aquí"], correct: 1 }
        ]
      }
    ]
  },
  { 
    id: "informatica", 
    name: "Informática Básica", 
    tagline: "Conceitos essenciais para usar o computador com confiança.", 
    color: "#7B4CA0", 
    soft: "#F1EAF6", 
    speechLang: null, 
    emoji: "💻",
    lessons: [
      {
        title: "Partes do Computador",
        intro: "Entenda os componentes principais que formam um computador desktop ou notebook.",
        vocab: [
          {term:"Monitor",translation:"Tela que exibe as imagens"},
          {term:"Teclado",translation:"Periférico de entrada para digitação"},
          {term:"Mouse",translation:"Dispositivo para apontar e clicar"},
          {term:"Gabinete/CPU",translation:"Unidade onde ficam o processador e memórias"},
          {term:"Mousepad",translation:"Superfície para deslizar o mouse"}
        ],
        quiz: [
          { q: "Qual componente exibe as imagens no computador?", options: ["Teclado", "Monitor", "Mouse", "Gabinete"], correct: 1 },
          { q: "Para digitar textos usamos o:", options: ["Mouse", "Monitor", "Teclado", "Fone"], correct: 2 },
          { q: "O dispositivo usado para mover o ponteiro na tela é:", options: ["Mouse", "Gabinete", "Impressora", "Estabilizador"], correct: 0 }
        ]
      },
      {
        title: "Sistema Operacional e Arquivos",
        intro: "Aprenda como gerenciar pastas, arquivos e navegar pela área de trabalho.",
        vocab: [
          {term:"Área de Trabalho",translation:"Tela inicial com ícones de acesso"},
          {term:"Pasta",translation:"Local para organizar arquivos"},
          {term:"Arquivo",translation:"Documento, imagem ou áudio salvo"},
          {term:"Lixeira",translation:"Local onde ficam arquivos excluídos"}
        ],
        quiz: [
          { q: "Onde ficam armazenados os arquivos excluídos temporariamente?", options: ["Pasta", "Lixeira", "Área de Trabalho", "Teclado"], correct: 1 },
          { q: "Para organizar vários documentos juntos usamos uma:", options: ["Lixeira", "Pasta", "Janela", "Rede"], correct: 1 }
        ]
      }
    ]
  }
];

/* ---------------- Gerenciamento de Estado ---------------- */
let currentView = "home"; // home, course, lesson, admin, student_login
let selectedCourse = null;
let selectedLessonIndex = 0;
let currentStudent = JSON.parse(localStorage.getItem("iml_student") || "null");

/* ---------------- Elementos DOM ---------------- */
const app = document.getElementById("app");
const logoImg = document.getElementById("logo-img");
const mainNav = document.getElementById("main-nav");
const homeBtn = document.getElementById("home-btn");
const studentInfo = document.getElementById("student-info");

/* Renderizar Logo */
logoImg.innerHTML = `<img src="${LOGO_SRC}" alt="IML Logo" style="width:100%;height:100%;object-fit:cover;" />`;

/* Event Listener para a marca */
homeBtn.addEventListener("click", () => {
  currentView = "home";
  render();
});

/* ---------------- Síntese de Voz (TTS) ---------------- */
function speak(text, lang) {
  if (!lang || !('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = lang;
  window.speechSynthesis.speak(utterance);
}

/* ---------------- Renderização Principal ---------------- */
function renderNav() {
  mainNav.innerHTML = `
    <button class="link ${currentView==='home'?'active':''}" onclick="navigate('home')">Cursos</button>
    <button class="link ${currentView==='student_login'?'active':''}" onclick="navigate('student_login')">
      ${currentStudent ? 'Perfil (' + currentStudent.name + ')' : 'Área do Aluno'}
    </button>
    <button class="link ${currentView==='admin'?'active':''}" onclick="navigate('admin')">Painel Admin</button>
  `;

  if (currentStudent) {
    studentInfo.style.display = "block";
    studentInfo.innerHTML = `Aluno ativo: <strong>${currentStudent.name}</strong> (${currentStudent.email})`;
  } else {
    studentInfo.style.display = "none";
  }
}

function navigate(view, course = null, lessonIndex = 0) {
  currentView = view;
  if (course) selectedCourse = course;
  selectedLessonIndex = lessonIndex;
  render();
}

function render() {
  renderNav();
  app.innerHTML = "";

  if (currentView === "home") renderHome();
  else if (currentView === "course") renderCourse();
  else if (currentView === "lesson") renderLesson();
  else if (currentView === "student_login") renderStudentLogin();
  else if (currentView === "admin") renderAdmin();
}

/* ---------------- Telas da Aplicação ---------------- */

// 1. Tela Inicial (Lista de Cursos)
function renderHome() {
  let html = `
    <div class="section max-w-5xl">
      <h1 style="font-size:24px;margin-bottom:8px">Cursos Disponíveis</h1>
      <p class="muted" style="margin-bottom:20px">Escolha uma modalidade abaixo para começar seus estudos.</p>
      <div class="grid cols-3">
  `;

  COURSES.forEach(c => {
    html += `
      <div class="course-card" onclick="navigate('course', COURSES.find(x => x.id === '${c.id}'))">
        <div class="topbar" style="background:${c.color}"></div>
        <div class="body">
          <div style="font-size:24px">${c.emoji}</div>
          <div style="font-weight:700;font-size:16px;color:var(--ink)">${c.name}</div>
          <div class="small">${c.tagline}</div>
          <div class="small" style="margin-top:auto;color:${c.color}"><strong>${c.lessons.length} aulas</strong></div>
        </div>
      </div>
    `;
  });

  html += `</div></div>`;
  app.innerHTML = html;
}

// 2. Tela de Detalhes do Curso
function renderCourse() {
  if (!selectedCourse) return navigate('home');

  let html = `
    <div class="section max-w-5xl">
      <button class="btn ghost" onclick="navigate('home')" style="margin-bottom:16px">← Voltar aos cursos</button>
      <div class="card" style="border-left:6px solid ${selectedCourse.color};margin-bottom:20px">
        <h1 style="margin:0 0 8px 0">${selectedCourse.emoji} ${selectedCourse.name}</h1>
        <p class="muted" style="margin:0">${selectedCourse.tagline}</p>
      </div>

      <h2 style="font-size:18px;margin-bottom:12px">Conteúdo do Curso</h2>
      <div style="border:1px solid var(--line);border-radius:10px;overflow:hidden">
  `;

  selectedCourse.lessons.forEach((l, idx) => {
    html += `
      <div class="lesson-row" onclick="navigate('lesson', selectedCourse, ${idx})">
        <div class="badge" style="background:${selectedCourse.soft};color:${selectedCourse.color}">${idx + 1}</div>
        <div style="flex:1">
          <div style="font-weight:600">${l.title}</div>
          <div class="small">${l.vocab ? l.vocab.length + ' palavras' : 'Conteúdo interativo'}</div>
        </div>
        <div>➔</div>
      </div>
    `;
  });

  html += `</div></div>`;
  app.innerHTML = html;
}

// 3. Tela da Lição (Vocabulário e Quiz)
function renderLesson() {
  if (!selectedCourse) return navigate('home');
  const lesson = selectedCourse.lessons[selectedLessonIndex];

  let html = `
    <div class="section max-w-5xl">
      <button class="btn ghost" onclick="navigate('course', selectedCourse)" style="margin-bottom:16px">← Voltar para ${selectedCourse.name}</button>
      
      <div class="card" style="margin-bottom:20px">
        <span class="small" style="color:${selectedCourse.color}">Lição ${selectedLessonIndex + 1} de ${selectedCourse.lessons.length}</span>
        <h1 style="margin:4px 0 12px 0">${lesson.title}</h1>
        <p style="margin:0;line-height:1.5">${lesson.intro}</p>
      </div>
  `;

  // Vocabulário (se houver)
  if (lesson.vocab && lesson.vocab.length > 0) {
    html += `
      <h2 style="font-size:18px;margin-bottom:12px">Vocabulário & Pronúncia</h2>
      <div class="grid" style="grid-template-columns:repeat(auto-fill, minmax(220px, 1fr));gap:12px;margin-bottom:28px">
    `;
    lesson.vocab.forEach(v => {
      const clickVoice = selectedCourse.speechLang ? `onclick="speak('${v.term.replace(/'/g, "\\'")}', '${selectedCourse.speechLang}')"` : '';
      html += `
        <div class="card" ${clickVoice} style="${selectedCourse.speechLang ? 'cursor:pointer' : ''}">
          <div style="font-weight:700;color:${selectedCourse.color}">${v.term} ${selectedCourse.speechLang ? '🔊' : ''}</div>
          <div class="small">${v.translation}</div>
        </div>
      `;
    });
    html += `</div>`;
  }

  // Quiz
  if (lesson.quiz && lesson.quiz.length > 0) {
    html += `
      <h2 style="font-size:18px;margin-bottom:12px">Exercício de Fixação</h2>
      <form id="quiz-form" onsubmit="handleQuizSubmit(event)">
    `;

    lesson.quiz.forEach((q, qIdx) => {
      html += `
        <div class="card" style="margin-bottom:16px">
          <div style="font-weight:600;margin-bottom:10px">${qIdx + 1}. ${q.q}</div>
          <div style="display:flex;flex-direction:column;gap:8px">
      `;

      q.options.forEach((opt, oIdx) => {
        html += `
          <label style="display:flex;align-items:center;gap:8px;font-size:14px;cursor:pointer">
            <input type="radio" name="q_${qIdx}" value="${oIdx}" required />
            <span>${opt}</span>
          </label>
        `;
      });

      html += `</div></div>`;
    });

    html += `
        <button type="submit" class="btn primary" style="background:${selectedCourse.color}">Enviar Respostas</button>
      </form>
      <div id="quiz-result" style="margin-top:16px"></div>
    `;
  }

  html += `</div>`;
  app.innerHTML = html;
}

// Submissão do Quiz
function handleQuizSubmit(e) {
  e.preventDefault();
  const lesson = selectedCourse.lessons[selectedLessonIndex];
  let score = 0;

  lesson.quiz.forEach((q, idx) => {
    const selected = document.querySelector(`input[name="q_${idx}"]:checked`);
    if (selected && parseInt(selected.value) === q.correct) {
      score++;
    }
  });

  const resultDiv = document.getElementById("quiz-result");
  const percent = Math.round((score / lesson.quiz.length) * 100);

  resultDiv.innerHTML = `
    <div class="card" style="border-color:${percent >= 70 ? '#3B8C6E' : '#C0392B'}">
      <div style="font-weight:700;font-size:16px" class="${percent >= 70 ? 'text-success' : 'text-danger'}">
        Você acertou ${score} de ${lesson.quiz.length} questões (${percent}%)
      </div>
      <p style="margin:4px 0 0 0;font-size:14px">
        ${percent >= 70 ? 'Parabéns! Excelente desempenho.' : 'Revise o conteúdo e tente novamente.'}
      </p>
    </div>
  `;

  // Salvar progresso se o aluno estiver logado
  if (currentStudent) {
    let progress = JSON.parse(localStorage.getItem(`iml_progress_${currentStudent.email}`) || "{}");
    progress[`${selectedCourse.id}_${selectedLessonIndex}`] = percent;
    localStorage.setItem(`iml_progress_${currentStudent.email}`, JSON.stringify(progress));
  }
}

// 4. Área do Aluno / Login
function renderStudentLogin() {
  if (currentStudent) {
    let progress = JSON.parse(localStorage.getItem(`iml_progress_${currentStudent.email}`) || "{}");
    
    let html = `
      <div class="section max-w-5xl">
        <div class="card" style="margin-bottom:20px">
          <h1 style="margin:0 0 8px 0">Perfil do Aluno</h1>
          <div><strong>Nome:</strong> ${currentStudent.name}</div>
          <div><strong>E-mail:</strong> ${currentStudent.email}</div>
          <button class="btn ghost" onclick="logoutStudent()" style="margin-top:12px;color:var(--text-danger)">Sair da conta</button>
        </div>

        <h2 style="font-size:18px;margin-bottom:12px">Seu Progresso</h2>
        <div class="card">
    `;

    if (Object.keys(progress).length === 0) {
      html += `<p class="muted" style="margin:0">Você ainda não concluiu nenhum exercício.</p>`;
    } else {
      html += `<table><thead><tr><th>Curso / Lição</th><th>Nota</th></tr></thead><tbody>`;
      for (let key in progress) {
        const [cId, lIdx] = key.split('_');
        const course = COURSES.find(c => c.id === cId);
        const lessonName = course ? course.lessons[lIdx]?.title : 'Lição';
        html += `
          <tr>
            <td><strong>${course ? course.name : cId}</strong> - ${lessonName}</td>
            <td class="${progress[key] >= 70 ? 'text-success' : 'text-danger'}"><strong>${progress[key]}%</strong></td>
          </tr>
        `;
      }
      html += `</tbody></table>`;
    }

    html += `</div></div>`;
    app.innerHTML = html;
    return;
  }

  // Formulário de Login de Aluno
  app.innerHTML = `
    <div class="section max-w-5xl" style="max-width:400px">
      <div class="card">
        <h1 style="font-size:20px;margin-bottom:16px">Área do Aluno</h1>
        <form onsubmit="loginStudent(event)">
          <div style="margin-bottom:12px">
            <label class="small">Seu Nome</label>
            <input type="text" id="std-name" required placeholder="Ex: Maria Silva" />
          </div>
          <div style="margin-bottom:16px">
            <label class="small">Seu E-mail</label>
            <input type="email" id="std-email" required placeholder="aluno@email.com" />
          </div>
          <button type="submit" class="btn primary" style="width:100%">Entrar / Registrar</button>
        </form>
      </div>
    </div>
  `;
}

function loginStudent(e) {
  e.preventDefault();
  const name = document.getElementById("std-name").value;
  const email = document.getElementById("std-email").value;
  currentStudent = { name, email };
  localStorage.setItem("iml_student", JSON.stringify(currentStudent));
  render();
}

function logoutStudent() {
  currentStudent = null;
  localStorage.removeItem("iml_student");
  render();
}

// 5. Painel do Administrador
function renderAdmin() {
  app.innerHTML = `
    <div class="section max-w-5xl">
      <h1 style="font-size:22px;margin-bottom:8px">Painel Administrativo</h1>
      <p class="muted" style="margin-bottom:20px">Gerenciamento de conteúdos e configurações do sistema.</p>
      
      <div class="grid cols-3" style="margin-bottom:20px">
        <div class="card">
          <div class="small">Total de Cursos</div>
          <div style="font-size:24px;font-weight:700">${COURSES.length}</div>
        </div>
        <div class="card">
          <div class="small">Total de Lições</div>
          <div style="font-size:24px;font-weight:700">${COURSES.reduce((a, b) => a + b.lessons.length, 0)}</div>
        </div>
        <div class="card">
          <div class="small">Status do Sistema</div>
          <div style="font-size:24px;font-weight:700;color:#3B8C6E">Ativo</div>
        </div>
      </div>

      <div class="card">
        <h2 style="font-size:16px;margin-top:0">Cursos Cadastrados</h2>
        <table>
          <thead>
            <tr>
              <th>Curso</th>
              <th>Aulas</th>
              <th>Idioma Áudio</th>
            </tr>
          </thead>
          <tbody>
            ${COURSES.map(c => `
              <tr>
                <td><strong>${c.name}</strong></td>
                <td>${c.lessons.length}</td>
                <td>${c.speechLang || 'N/A'}</td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    </div>
  `;
}

/* Inicialização da aplicação */
render();
</script>
</body>
</html>
