<!-- index.html -->
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Examen Dinámico | Mecánica de Fluidos</title>

  <style>
    :root {
      --bg1: #06111f;
      --bg2: #111827;
      --card: rgba(255, 255, 255, 0.09);
      --card2: rgba(255, 255, 255, 0.14);
      --border: rgba(255, 255, 255, 0.16);
      --text: #f8fafc;
      --muted: #a9b4c7;
      --blue: #38bdf8;
      --green: #22c55e;
      --red: #ef4444;
      --yellow: #f59e0b;
      --purple: #a78bfa;
      --shadow: 0 28px 90px rgba(0, 0, 0, 0.38);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at top left, rgba(56, 189, 248, 0.25), transparent 34%),
        radial-gradient(circle at bottom right, rgba(167, 139, 250, 0.23), transparent 32%),
        linear-gradient(135deg, var(--bg1), var(--bg2));
      overflow-x: hidden;
    }

    .container {
      width: min(1180px, calc(100% - 28px));
      margin: auto;
      padding: 28px 0 44px;
    }

    .glass {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 30px;
      box-shadow: var(--shadow);
      backdrop-filter: blur(18px);
    }

    .hero {
      display: grid;
      grid-template-columns: 1.45fr 0.55fr;
      gap: 20px;
      margin-bottom: 20px;
    }

    .hero-main {
      padding: 30px;
      overflow: hidden;
      position: relative;
    }

    .hero-main::after {
      content: "";
      position: absolute;
      width: 230px;
      height: 230px;
      border-radius: 50%;
      top: -80px;
      right: -60px;
      background: rgba(56, 189, 248, 0.14);
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 9px 13px;
      border-radius: 999px;
      border: 1px solid var(--border);
      background: rgba(255, 255, 255, 0.08);
      color: #dbeafe;
      font-size: 13px;
      font-weight: 800;
      letter-spacing: 0.2px;
    }

    h1 {
      margin: 16px 0 10px;
      font-size: clamp(32px, 5vw, 56px);
      line-height: 0.98;
      letter-spacing: -1.8px;
    }

    .hero-main p {
      margin: 0;
      max-width: 760px;
      line-height: 1.55;
      color: var(--muted);
      font-size: 16px;
    }

    .hero-stats {
      padding: 20px;
      display: grid;
      gap: 14px;
    }

    .stat {
      padding: 17px;
      border-radius: 24px;
      background: rgba(255, 255, 255, 0.075);
      border: 1px solid var(--border);
    }

    .stat small {
      display: block;
      margin-bottom: 8px;
      color: var(--muted);
      font-weight: 700;
    }

    .stat strong {
      font-size: 28px;
    }

    .setup {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr auto;
      gap: 13px;
      padding: 18px;
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-bottom: 8px;
      font-size: 12px;
      color: var(--muted);
      font-weight: 900;
      text-transform: uppercase;
      letter-spacing: 0.8px;
    }

    input,
    select,
    button {
      width: 100%;
      border: 0;
      outline: 0;
      font: inherit;
      border-radius: 17px;
      padding: 14px 15px;
    }

    input,
    select {
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid var(--border);
      color: var(--text);
    }

    select option {
      color: white;
      background: #111827;
    }

    button {
      cursor: pointer;
      font-weight: 950;
      color: #04111f;
      background: linear-gradient(135deg, var(--blue), var(--green));
      box-shadow: 0 18px 36px rgba(34, 197, 94, 0.17);
      transition: 0.2s ease;
    }

    button:hover {
      transform: translateY(-2px);
      filter: brightness(1.05);
    }

    button.secondary {
      background: rgba(255, 255, 255, 0.11);
      color: var(--text);
      border: 1px solid var(--border);
      box-shadow: none;
    }

    button.danger {
      background: linear-gradient(135deg, #fb7185, #f97316);
      color: #190308;
    }

    button:disabled {
      opacity: 0.45;
      cursor: not-allowed;
      transform: none;
    }

    .exam {
      display: grid;
      grid-template-columns: 320px 1fr;
      gap: 20px;
      align-items: start;
    }

    .sidebar {
      padding: 18px;
      position: sticky;
      top: 16px;
    }

    .progress-box {
      height: 13px;
      margin: 10px 0 18px;
      border-radius: 999px;
      overflow: hidden;
      background: rgba(255, 255, 255, 0.11);
    }

    .progress {
      height: 100%;
      width: 0%;
      border-radius: inherit;
      background: linear-gradient(90deg, var(--blue), var(--green));
      transition: width 0.35s ease;
    }

    .timer {
      margin-top: 14px;
      padding: 15px;
      border-radius: 20px;
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid var(--border);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .timer strong {
      font-size: 24px;
      color: #fef3c7;
    }

    .mini-grid {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 8px;
      margin-top: 14px;
    }

    .mini {
      aspect-ratio: 1;
      border-radius: 13px;
      border: 1px solid var(--border);
      background: rgba(255, 255, 255, 0.08);
      display: grid;
      place-items: center;
      font-weight: 950;
      cursor: pointer;
      transition: 0.2s;
    }

    .mini.active {
      background: rgba(56, 189, 248, 0.17);
      outline: 2px solid var(--blue);
    }

    .mini.answered {
      background: rgba(34, 197, 94, 0.18);
      border-color: rgba(34, 197, 94, 0.42);
    }

    .question-card {
      min-height: 610px;
      padding: 27px;
    }

    .topline {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: space-between;
      margin-bottom: 18px;
    }

    .chips {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .chip {
      padding: 8px 11px;
      border-radius: 999px;
      background: rgba(255, 255, 255, 0.09);
      border: 1px solid var(--border);
      color: #dbeafe;
      font-size: 13px;
      font-weight: 850;
    }

    .question {
      margin: 16px 0 22px;
      font-size: clamp(22px, 3vw, 34px);
      line-height: 1.18;
      letter-spacing: -0.5px;
    }

    .options {
      display: grid;
      gap: 12px;
    }

    .option {
      display: grid;
      grid-template-columns: 43px 1fr;
      align-items: center;
      gap: 14px;
      padding: 16px;
      border-radius: 23px;
      border: 1px solid var(--border);
      background: rgba(255, 255, 255, 0.075);
      cursor: pointer;
      transition: 0.18s ease;
    }

    .option:hover {
      transform: translateX(4px);
      background: rgba(255, 255, 255, 0.12);
    }

    .letter {
      width: 43px;
      height: 43px;
      border-radius: 15px;
      display: grid;
      place-items: center;
      font-weight: 1000;
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid var(--border);
    }

    .option.selected {
      background: rgba(56, 189, 248, 0.16);
      border-color: rgba(56, 189, 248, 0.9);
    }

    .option.correct {
      background: rgba(34, 197, 94, 0.17);
      border-color: rgba(34, 197, 94, 0.9);
    }

    .option.wrong {
      background: rgba(239, 68, 68, 0.16);
      border-color: rgba(239, 68, 68, 0.9);
    }

    .feedback {
      display: none;
      margin-top: 16px;
      padding: 18px;
      border-radius: 22px;
      border: 1px solid var(--border);
      background: rgba(255, 255, 255, 0.08);
      color: #dbeafe;
      line-height: 1.55;
    }

    .feedback.show {
      display: block;
      animation: pop 0.25s ease;
    }

    @keyframes pop {
      from {
        opacity: 0;
        transform: translateY(8px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .actions {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
      margin-top: 18px;
    }

    .results {
      display: none;
      padding: 28px;
    }

    .results.show {
      display: block;
    }

    .score-layout {
      display: grid;
      grid-template-columns: 225px 1fr;
      gap: 25px;
      align-items: center;
    }

    .circle {
      width: 225px;
      height: 225px;
      border-radius: 50%;
      display: grid;
      place-items: center;
      background:
        radial-gradient(circle at center, #0f172a 58%, transparent 59%),
        conic-gradient(var(--green) 0deg, var(--green) var(--deg), rgba(255,255,255,0.12) var(--deg), rgba(255,255,255,0.12) 360deg);
      border: 1px solid var(--border);
    }

    .circle strong {
      font-size: 46px;
    }

    .review {
      display: grid;
      gap: 12px;
      margin-top: 22px;
    }

    .review-item {
      padding: 17px;
      border-radius: 22px;
      background: rgba(255, 255, 255, 0.075);
      border: 1px solid var(--border);
    }

    .review-item h3 {
      margin: 0 0 9px;
      font-size: 16px;
      line-height: 1.35;
    }

    .review-item p {
      margin: 5px 0;
      color: var(--muted);
      line-height: 1.45;
    }

    .hidden {
      display: none !important;
    }

    .floating {
      position: fixed;
      right: 18px;
      bottom: 18px;
      width: 54px;
      height: 54px;
      border-radius: 18px;
      display: grid;
      place-items: center;
      background: rgba(56, 189, 248, 0.18);
      border: 1px solid rgba(56, 189, 248, 0.45);
      box-shadow: var(--shadow);
      backdrop-filter: blur(16px);
      animation: float 2.5s ease-in-out infinite;
      z-index: 10;
    }

    @keyframes float {
      0%, 100% {
        transform: translateY(0);
      }
      50% {
        transform: translateY(-8px);
      }
    }

    @media (max-width: 900px) {
      .hero,
      .setup,
      .exam,
      .score-layout {
        grid-template-columns: 1fr;
      }

      .sidebar {
        position: static;
      }

      .actions {
        grid-template-columns: 1fr;
      }

      .circle {
        margin: auto;
      }
    }
  </style>
</head>

<body>
  <div class="floating">💧</div>

  <main class="container">
    <section class="hero">
      <div class="hero-main glass">
        <span class="badge">⚙️ Examen dinámico de Mecánica de Fluidos</span>
        <h1>Evaluación interactiva con alternativas difíciles</h1>
        <p>
          Examen con preguntas de análisis sobre presión, masa, peso, unidades, sistemas,
          volumen de control, compresibilidad, densidad, gravedad específica, tensión superficial,
          número de Mach y conceptos fundamentales de mecánica de fluidos.
        </p>
      </div>

      <div class="hero-stats glass">
        <div class="stat">
          <small>Banco de preguntas</small>
          <strong id="totalBank">30</strong>
        </div>
        <div class="stat">
          <small>Dificultad</small>
          <strong>Alta</strong>
        </div>
        <div class="stat">
          <small>Formato</small>
          <strong>GitHub Pages</strong>
        </div>
      </div>
    </section>

    <section class="setup glass" id="setup">
      <div>
        <label>Nombre del estudiante</label>
        <input id="studentName" placeholder="Ingrese nombres y apellidos" />
      </div>

      <div>
        <label>Número de preguntas</label>
        <select id="questionCount">
          <option value="10">10 preguntas</option>
          <option value="15" selected>15 preguntas</option>
          <option value="20">20 preguntas</option>
          <option value="30">30 preguntas</option>
        </select>
      </div>

      <div>
        <label>Tiempo</label>
        <select id="timeLimit">
          <option value="10">10 minutos</option>
          <option value="15" selected>15 minutos</option>
          <option value="20">20 minutos</option>
          <option value="30">30 minutos</option>
        </select>
      </div>

      <div style="display:flex;align-items:end;">
        <button onclick="startExam()">Iniciar examen</button>
      </div>
    </section>

    <section class="exam hidden" id="exam">
      <aside class="sidebar glass">
        <label>Progreso</label>
        <div class="progress-box">
          <div class="progress" id="progress"></div>
        </div>

        <div class="stat">
          <small>Estudiante</small>
          <strong id="studentLabel" style="font-size:18px;">---</strong>
        </div>

        <div class="timer">
          <span>⏱ Tiempo</span>
          <strong id="timer">15:00</strong>
        </div>

        <div class="mini-grid" id="miniGrid"></div>
      </aside>

      <section class="question-card glass">
        <div class="topline">
          <div class="chips">
            <span class="chip" id="qNumber">Pregunta 1</span>
            <span class="chip" id="qTopic">Tema</span>
            <span class="chip" id="qLevel">Nivel</span>
          </div>

          <span class="chip" id="answeredCount">0 respondidas</span>
        </div>

        <h2 class="question" id="questionText"></h2>

        <div class="options" id="options"></div>

        <div class="feedback" id="feedback"></div>

        <div class="actions">
          <button class="secondary" onclick="previousQuestion()">Anterior</button>
          <button onclick="checkQuestion()" id="checkBtn">Verificar</button>
          <button class="secondary" onclick="nextQuestion()">Siguiente</button>
        </div>

        <div class="actions">
          <button class="danger" onclick="finishExam()">Finalizar</button>
          <button class="secondary" onclick="toggleExplanations()">Explicaciones</button>
          <button class="secondary" onclick="restartExam()">Reiniciar</button>
        </div>
      </section>
    </section>

    <section class="results glass" id="results">
      <div class="score-layout">
        <div class="circle" id="circle" style="--deg:0deg;">
          <strong id="scorePercent">0%</strong>
        </div>

        <div>
          <span class="badge">📊 Resultado final</span>
          <h1 id="finalTitle">Examen finalizado</h1>
          <p id="finalSummary" style="color:var(--muted);line-height:1.6;"></p>

          <div class="actions">
            <button onclick="downloadReport()">Descargar reporte</button>
            <button class="secondary" onclick="restartExam()">Nuevo intento</button>
            <button class="secondary" onclick="window.print()">Imprimir</button>
          </div>
        </div>
      </div>

      <div class="review" id="review"></div>
    </section>
  </main>

  <script>
    const QUESTION_BANK = [
      {
        id: 1,
        topic: "Naturaleza de los fluidos",
        level: "Conceptual avanzado",
        question: "Desde la perspectiva mecánica, ¿qué criterio permite distinguir rigurosamente a un fluido de un sólido elástico cuando ambos son sometidos a esfuerzo cortante?",
        options: [
          "El fluido puede soportar esfuerzos normales, pero no esfuerzos tangenciales.",
          "El fluido se deforma continuamente bajo cualquier esfuerzo cortante, mientras el sólido puede alcanzar equilibrio deformado.",
          "El sólido cambia de volumen ante presión, mientras el fluido conserva siempre volumen constante.",
          "El fluido solo se deforma cuando el esfuerzo cortante supera su módulo volumétrico."
        ],
        answer: 1,
        explanation: "Un fluido no permanece en equilibrio estático bajo esfuerzo cortante; se deforma continuamente aunque el esfuerzo sea pequeño."
      },
      {
        id: 2,
        topic: "Líquidos y gases",
        level: "Conceptual avanzado",
        question: "Un gas confinado en un recipiente con pistón móvil reduce notablemente su volumen cuando aumenta la presión. ¿Qué conclusión técnica es correcta?",
        options: [
          "Los gases son prácticamente incompresibles porque llenan todo el recipiente.",
          "Los gases son fácilmente compresibles, a diferencia de los líquidos que cambian muy poco su volumen.",
          "La compresibilidad de gases y líquidos es equivalente si ambos están a presión atmosférica.",
          "El gas reduce su volumen únicamente porque disminuye su masa."
        ],
        answer: 1,
        explanation: "Los gases son fácilmente compresibles; los líquidos solo son ligeramente compresibles."
      },
      {
        id: 3,
        topic: "Presión",
        level: "Cálculo aplicado",
        question: "Un pistón aplica una fuerza perpendicular de 12.0 kN sobre aceite. Si el diámetro del pistón es 75 mm, ¿cuál es la presión aproximada en el aceite?",
        options: [
          "2.72 MPa",
          "0.213 MPa",
          "27.2 MPa",
          "21.3 kPa"
        ],
        answer: 0,
        explanation: "A = πD²/4 = π(0.075)²/4 = 0.004418 m². p = F/A = 12000/0.004418 ≈ 2.72 MPa."
      },
      {
        id: 4,
        topic: "Leyes de Pascal",
        level: "Conceptual avanzado",
        question: "¿Cuál afirmación describe correctamente el comportamiento de la presión en un fluido confinado por fronteras sólidas?",
        options: [
          "La presión actúa únicamente en la dirección de la fuerza aplicada originalmente.",
          "La presión actúa paralela a las paredes si el fluido está en movimiento.",
          "La presión actúa perpendicularmente a las fronteras sólidas y se transmite en todas las direcciones.",
          "La presión disminuye automáticamente en las paredes laterales por efecto de la viscosidad."
        ],
        answer: 2,
        explanation: "La presión en un fluido confinado actúa en todas las direcciones y perpendicularmente a las fronteras."
      },
      {
        id: 5,
        topic: "Sistema Internacional",
        level: "Análisis dimensional",
        question: "En el SI, ¿por qué el newton puede expresarse como kg·m/s²?",
        options: [
          "Porque la fuerza es una magnitud básica independiente de la masa.",
          "Porque se deriva de F = ma, donde la masa está en kg y la aceleración en m/s².",
          "Porque el peso específico se define como fuerza por unidad de volumen.",
          "Porque el pascal es equivalente a N/m²."
        ],
        answer: 1,
        explanation: "La unidad de fuerza se obtiene de F = ma. Por ello, 1 N = 1 kg·m/s²."
      },
      {
        id: 6,
        topic: "Peso y masa",
        level: "Conceptual difícil",
        question: "¿Cuál es la diferencia técnica más precisa entre masa y peso?",
        options: [
          "La masa es una fuerza y el peso es una propiedad escalar independiente de la gravedad.",
          "La masa mide la inercia o cantidad de materia; el peso es la fuerza gravitatoria ejercida sobre esa masa.",
          "La masa solo existe en el SI y el peso solo existe en el sistema inglés.",
          "El peso permanece constante en cualquier planeta, mientras la masa cambia con la gravedad."
        ],
        answer: 1,
        explanation: "La masa mide cantidad de materia o inercia; el peso depende de la gravedad: w = mg."
      },
      {
        id: 7,
        topic: "Peso y masa",
        level: "Cálculo aplicado",
        question: "Un componente tiene masa de 150 kg en un lugar donde g = 9.6 m/s². ¿Cuál es su peso?",
        options: [
          "15.625 N",
          "1440 N",
          "1562.5 N",
          "14.4 kN"
        ],
        answer: 1,
        explanation: "w = mg = 150 × 9.6 = 1440 N."
      },
      {
        id: 8,
        topic: "Sistema inglés",
        level: "Conceptual difícil",
        question: "En el sistema inglés gravitacional coherente, ¿cuál es la unidad derivada de masa?",
        options: [
          "lbf",
          "lbm",
          "slug",
          "psi"
        ],
        answer: 2,
        explanation: "En el sistema inglés gravitacional, la unidad coherente de masa es el slug."
      },
      {
        id: 9,
        topic: "lbm y lbf",
        level: "Conceptual difícil",
        question: "¿Por qué la equivalencia numérica entre lbm y lbf solo es válida bajo gravedad estándar?",
        options: [
          "Porque lbm y lbf son siempre la misma unidad física.",
          "Porque la relación peso-masa en lbm requiere la constante gc y depende del valor local de g.",
          "Porque la masa cambia con la aceleración de la gravedad.",
          "Porque la libra-fuerza solo se usa para fluidos líquidos."
        ],
        answer: 1,
        explanation: "Con lbm se usa F = m(a/gc). Si g cambia, el peso en lbf ya no coincide numéricamente con la masa en lbm."
      },
      {
        id: 10,
        topic: "lbm y lbf",
        level: "Caso aplicado difícil",
        question: "Un astronauta de 195 lbm está en la Luna, donde g = 5.48 ft/s². ¿Qué leería aproximadamente una báscula de resorte?",
        options: [
          "195 lbf",
          "33.2 lbf",
          "5.48 lbf",
          "1176 lbf"
        ],
        answer: 1,
        explanation: "w = m(g/gc) = 195(5.48/32.174) ≈ 33.2 lbf."
      },
      {
        id: 11,
        topic: "Volumen de control",
        level: "Conceptual avanzado",
        question: "Al analizar el flujo de gas a través de una boquilla, ¿qué tipo de sistema resulta más adecuado?",
        options: [
          "Sistema cerrado, porque la masa dentro de la boquilla permanece fija.",
          "Volumen de control, porque la masa cruza las fronteras de entrada y salida.",
          "Sistema aislado, porque no hay transferencia de energía.",
          "Sistema rígido, porque el fluido no cambia de velocidad."
        ],
        answer: 1,
        explanation: "Una boquilla se analiza como volumen de control porque hay flujo de masa a través de sus fronteras."
      },
      {
        id: 12,
        topic: "Sistema cerrado y volumen de control",
        level: "Conceptual difícil",
        question: "¿Qué criterio diferencia esencialmente a un sistema cerrado de un volumen de control?",
        options: [
          "La transferencia de calor a través de la frontera.",
          "La posibilidad de que la masa cruce la frontera.",
          "La existencia de presión atmosférica.",
          "La presencia de trabajo de eje."
        ],
        answer: 1,
        explanation: "En un sistema cerrado no cruza masa; en un volumen de control sí puede entrar y salir masa."
      },
      {
        id: 13,
        topic: "Flujo estacionario",
        level: "Conceptual difícil",
        question: "¿Cuál definición corresponde a un proceso de flujo estacionario?",
        options: [
          "La masa total del sistema aumenta con el tiempo de forma constante.",
          "Las propiedades en un punto espacial no cambian respecto al tiempo.",
          "La velocidad del fluido es cero en todo el dominio.",
          "La presión disminuye linealmente en la dirección del flujo."
        ],
        answer: 1,
        explanation: "En flujo estacionario, las propiedades en cada punto espacial no varían con el tiempo."
      },
      {
        id: 14,
        topic: "Flujo incompresible",
        level: "Criterio técnico",
        question: "En un sistema de ventilación con aire a Ma = 0.12, ¿por qué puede aplicarse el modelo de flujo incompresible?",
        options: [
          "Porque todo flujo de aire es incompresible a presión atmosférica.",
          "Porque Ma < 0.3 implica variaciones de densidad pequeñas, usualmente menores al 5%.",
          "Porque la viscosidad del aire desaparece a bajo Mach.",
          "Porque el régimen laminar garantiza densidad constante."
        ],
        answer: 1,
        explanation: "Para Ma < 0.3, los cambios de densidad suelen ser despreciables para análisis incompresible."
      },
      {
        id: 15,
        topic: "Número de Mach",
        level: "Conceptual aplicado",
        question: "Si un flujo de vapor en una tubería tiene Ma = 2, ¿qué interpretación física es correcta?",
        options: [
          "La presión del vapor es el doble de la presión atmosférica.",
          "La velocidad del fluido es el doble de la velocidad local del sonido.",
          "La densidad del fluido es dos veces la densidad crítica.",
          "La viscosidad cinemática se duplica por compresibilidad."
        ],
        answer: 1,
        explanation: "Ma = V/c. Si Ma = 2, la velocidad del flujo es dos veces la velocidad local del sonido."
      },
      {
        id: 16,
        topic: "Capa límite",
        level: "Conceptual avanzado",
        question: "¿Cuál es la causa física principal del desarrollo de la capa límite sobre una superficie sólida?",
        options: [
          "La presión hidrostática uniforme en la pared.",
          "La condición de no deslizamiento generada por la viscosidad.",
          "La compresibilidad del fluido a cualquier velocidad.",
          "La tensión superficial entre líquido y gas."
        ],
        answer: 1,
        explanation: "La viscosidad impone no deslizamiento en la pared, creando gradientes de velocidad y crecimiento de capa límite."
      },
      {
        id: 17,
        topic: "Esfuerzo cortante y presión",
        level: "Conceptual difícil",
        question: "¿Cuál afirmación define correctamente esfuerzo cortante y presión hidrostática?",
        options: [
          "El esfuerzo cortante es normal al área y la presión es tangencial a la pared.",
          "El esfuerzo cortante es fuerza tangencial por unidad de área; la presión es esfuerzo normal en un fluido en reposo.",
          "Ambos son propiedades termodinámicas independientes del área.",
          "La presión solo existe en fluidos viscosos y el esfuerzo cortante solo en gases."
        ],
        answer: 1,
        explanation: "El esfuerzo cortante es tangencial; la presión hidrostática es normal a la superficie."
      },
      {
        id: 18,
        topic: "Consistencia dimensional",
        level: "Análisis dimensional",
        question: "Se obtiene la ecuación E = 16 kJ + 7 kJ/kg. ¿Cuál es el error dimensional?",
        options: [
          "No hay error, porque ambos términos contienen kJ.",
          "El primer término debe dividirse entre la masa para obtener kJ/kg.",
          "No pueden sumarse energía total y energía específica; el segundo término debe multiplicarse por masa si se desea energía total.",
          "El segundo término debe convertirse a kN para que sea compatible."
        ],
        answer: 2,
        explanation: "kJ y kJ/kg no son unidades homogéneas. Para sumar energía total, 7 kJ/kg debe multiplicarse por una masa."
      },
      {
        id: 19,
        topic: "Densidad",
        level: "Conceptual",
        question: "¿Cuál es la definición técnica de densidad?",
        options: [
          "Peso por unidad de volumen.",
          "Masa por unidad de volumen.",
          "Fuerza por unidad de área.",
          "Peso relativo respecto al aire."
        ],
        answer: 1,
        explanation: "La densidad se define como ρ = m/V."
      },
      {
        id: 20,
        topic: "Peso específico",
        level: "Conceptual",
        question: "¿Cuál es la definición técnica de peso específico?",
        options: [
          "Masa por unidad de volumen.",
          "Peso por unidad de volumen.",
          "Presión por unidad de profundidad.",
          "Densidad dividida entre gravedad."
        ],
        answer: 1,
        explanation: "El peso específico se define como γ = w/V."
      },
      {
        id: 21,
        topic: "Densidad y peso específico",
        level: "Cálculo aplicado",
        question: "La glicerina tiene gravedad específica sg = 1.263. Usando ρagua = 1000 kg/m³, ¿cuál es su densidad?",
        options: [
          "126.3 kg/m³",
          "1263 kg/m³",
          "12.63 kg/m³",
          "0.001263 kg/m³"
        ],
        answer: 1,
        explanation: "ρ = sg × ρagua = 1.263 × 1000 = 1263 kg/m³."
      },
      {
        id: 22,
        topic: "Densidad y peso específico",
        level: "Cálculo aplicado",
        question: "Un aceite tiene masa de 825 kg y volumen de 0.917 m³. ¿Cuál es su densidad aproximada?",
        options: [
          "900 kg/m³",
          "0.00111 kg/m³",
          "8.83 kN/m³",
          "917 kg/m³"
        ],
        answer: 0,
        explanation: "ρ = m/V = 825/0.917 ≈ 900 kg/m³."
      },
      {
        id: 23,
        topic: "Gravedad específica",
        level: "Conceptual avanzado",
        question: "La gravedad específica de una sustancia se define como:",
        options: [
          "La relación entre su presión y la presión atmosférica.",
          "La relación entre su densidad o peso específico y los valores correspondientes del agua a 4 °C.",
          "La relación entre su masa y su peso.",
          "La relación entre su volumen inicial y final al comprimirse."
        ],
        answer: 1,
        explanation: "sg = ρs/ρagua a 4 °C = γs/γagua a 4 °C."
      },
      {
        id: 24,
        topic: "Compresibilidad",
        level: "Conceptual avanzado",
        question: "¿Qué representa el módulo volumétrico de elasticidad de un líquido?",
        options: [
          "La relación entre esfuerzo cortante y deformación angular.",
          "La resistencia del fluido a cambiar su volumen ante cambios de presión.",
          "La facilidad con la que un fluido fluye por una tubería.",
          "La relación entre tensión superficial y radio capilar."
        ],
        answer: 1,
        explanation: "El módulo volumétrico mide la presión requerida para producir un cambio relativo de volumen."
      },
      {
        id: 25,
        topic: "Compresibilidad",
        level: "Cálculo difícil",
        question: "El agua tiene módulo volumétrico aproximado E = 316000 psi. ¿Qué cambio de presión se requiere para reducir su volumen en 1.0%?",
        options: [
          "316 psi",
          "3160 psi",
          "31600 psi",
          "31.6 psi"
        ],
        answer: 1,
        explanation: "Δp = -E(ΔV/V). Para ΔV/V = -0.01: Δp = 316000 × 0.01 = 3160 psi."
      },
      {
        id: 26,
        topic: "Tensión superficial",
        level: "Conceptual avanzado",
        question: "¿Cuál afirmación explica mejor por qué una aguja pequeña puede apoyarse sobre agua quieta y hundirse al agregar detergente?",
        options: [
          "El detergente aumenta la densidad del agua hasta superar la del metal.",
          "El detergente reduce la tensión superficial que sostenía parcialmente la aguja.",
          "El detergente transforma el agua en un fluido no viscoso.",
          "La aguja flota por empuje hidrostático y el detergente elimina la gravedad."
        ],
        answer: 1,
        explanation: "El detergente reduce la tensión superficial, por lo que la superficie ya no puede sostener la aguja."
      },
      {
        id: 27,
        topic: "Tensión superficial",
        level: "Unidades",
        question: "¿Cuáles son unidades coherentes para la tensión superficial?",
        options: [
          "N/m o lb/ft",
          "N/m² o psi",
          "kg/m³ o slugs/ft³",
          "N·m o ft·lb"
        ],
        answer: 0,
        explanation: "La tensión superficial se expresa como fuerza por unidad de longitud: N/m o lb/ft."
      },
      {
        id: 28,
        topic: "Presión hidráulica",
        level: "Cálculo difícil",
        question: "Un cilindro hidráulico debe ejercer 38.8 kN con un pistón de 40 mm de diámetro. ¿Qué presión aproximada requiere el aceite?",
        options: [
          "3.09 MPa",
          "30.9 MPa",
          "309 kPa",
          "0.0309 MPa"
        ],
        answer: 1,
        explanation: "A = π(0.04)²/4 = 0.001257 m². p = 38800/0.001257 ≈ 30.9 MPa."
      },
      {
        id: 29,
        topic: "Flujo interno y canal abierto",
        level: "Conceptual difícil",
        question: "¿Qué criterio técnico transforma el análisis de una conducción circular desde flujo interno presurizado hacia flujo en canal abierto?",
        options: [
          "La tubería cambia de material metálico a plástico.",
          "Aparece una superficie libre donde la presión coincide con la presión atmosférica local.",
          "El número de Reynolds supera el valor crítico.",
          "El fluido cambia de líquido a gas."
        ],
        answer: 1,
        explanation: "El flujo en canal abierto se caracteriza por la existencia de superficie libre sometida a presión atmosférica."
      },
      {
        id: 30,
        topic: "Clasificación de flujos",
        level: "Aplicación conceptual",
        question: "En una aeronave, el flujo sobre el extradós del ala y el flujo dentro de una turbina se clasifican respectivamente como:",
        options: [
          "Interno e interno.",
          "Externo e interno.",
          "Interno y externo.",
          "Canal abierto y flujo libre."
        ],
        answer: 1,
        explanation: "Sobre el ala el flujo no está confinado: externo. Dentro de la turbina está limitado por fronteras sólidas: interno."
      }
    ];

    let examQuestions = [];
    let current = 0;
    let answers = {};
    let checked = {};
    let showExplanations = true;
    let remainingSeconds = 0;
    let timerInterval = null;
    let startTime = null;

    const letters = ["A", "B", "C", "D"];

    document.getElementById("totalBank").textContent = QUESTION_BANK.length;

    function shuffle(array) {
      const copy = [...array];
      for (let i = copy.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [copy[i], copy[j]] = [copy[j], copy[i]];
      }
      return copy;
    }

    function prepareQuestions(count) {
      const selected = shuffle(QUESTION_BANK).slice(0, count);

      return selected.map((q) => {
        const mappedOptions = q.options.map((text, index) => ({
          text,
          correct: index === q.answer
        }));

        const mixed = shuffle(mappedOptions);
        const newAnswer = mixed.findIndex((option) => option.correct);

        return {
          ...q,
          options: mixed.map((option) => option.text),
          answer: newAnswer
        };
      });
    }

    function startExam() {
      const studentName = document.getElementById("studentName").value.trim();
      const count = Number(document.getElementById("questionCount").value);
      const minutes = Number(document.getElementById("timeLimit").value);

      if (!studentName) {
        alert("Ingrese el nombre del estudiante.");
        return;
      }

      examQuestions = prepareQuestions(count);
      current = 0;
      answers = {};
      checked = {};
      showExplanations = true;
      remainingSeconds = minutes * 60;
      startTime = new Date();

      document.getElementById("studentLabel").textContent = studentName;
      document.getElementById("setup").classList.add("hidden");
      document.getElementById("results").classList.remove("show");
      document.getElementById("exam").classList.remove("hidden");

      buildMiniGrid();
      renderQuestion();
      startTimer();
    }

    function startTimer() {
      clearInterval(timerInterval);
      updateTimer();

      timerInterval = setInterval(() => {
        remainingSeconds--;
        updateTimer();

        if (remainingSeconds <= 0) {
          clearInterval(timerInterval);
          finishExam();
        }
      }, 1000);
    }

    function updateTimer() {
      const safeTime = Math.max(0, remainingSeconds);
      const min = Math.floor(safeTime / 60);
      const sec = safeTime % 60;

      document.getElementById("timer").textContent =
        String(min).padStart(2, "0") + ":" + String(sec).padStart(2, "0");
    }

    function buildMiniGrid() {
      const grid = document.getElementById("miniGrid");
      grid.innerHTML = "";

      examQuestions.forEach((_, index) => {
        const item = document.createElement("div");
        item.className = "mini";
        item.textContent = index + 1;
        item.onclick = () => {
          current = index;
          renderQuestion();
        };

        grid.appendChild(item);
      });
    }

    function renderQuestion() {
      const q = examQuestions[current];

      document.getElementById("qNumber").textContent = `Pregunta ${current + 1} de ${examQuestions.length}`;
      document.getElementById("qTopic").textContent = q.topic;
      document.getElementById("qLevel").textContent = q.level;
      document.getElementById("questionText").textContent = q.question;

      const optionsContainer = document.getElementById("options");
      optionsContainer.innerHTML = "";

      q.options.forEach((option, index) => {
        const optionBox = document.createElement("div");
        optionBox.className = "option";

        if (answers[q.id] === index) optionBox.classList.add("selected");

        if (checked[q.id]) {
          if (index === q.answer) optionBox.classList.add("correct");
          if (answers[q.id] === index && index !== q.answer) optionBox.classList.add("wrong");
        }

        optionBox.onclick = () => selectOption(index);

        optionBox.innerHTML = `
          <div class="letter">${letters[index]}</div>
          <div>${option}</div>
        `;

        optionsContainer.appendChild(optionBox);
      });

      const feedback = document.getElementById("feedback");

      if (checked[q.id] && showExplanations) {
        const isCorrect = answers[q.id] === q.answer;
        feedback.classList.add("show");
        feedback.innerHTML = `
          <strong>${isCorrect ? "✅ Correcto" : "❌ Incorrecto"}</strong><br>
          <span>${q.explanation}</span>
        `;
      } else {
        feedback.classList.remove("show");
        feedback.innerHTML = "";
      }

      updateProgress();
      updateMiniGrid();
    }

    function selectOption(index) {
      const q = examQuestions[current];

      if (checked[q.id]) return;

      answers[q.id] = index;
      renderQuestion();
    }

    function checkQuestion() {
      const q = examQuestions[current];

      if (answers[q.id] === undefined) {
        alert("Seleccione una alternativa antes de verificar.");
        return;
      }

      checked[q.id] = true;
      renderQuestion();
    }

    function previousQuestion() {
      if (current > 0) {
        current--;
        renderQuestion();
      }
    }

    function nextQuestion() {
      if (current < examQuestions.length - 1) {
        current++;
        renderQuestion();
      }
    }

    function toggleExplanations() {
      showExplanations = !showExplanations;
      renderQuestion();
    }

    function updateProgress() {
      const answered = Object.keys(answers).length;
      const percent = (answered / examQuestions.length) * 100;

      document.getElementById("progress").style.width = percent + "%";
      document.getElementById("answeredCount").textContent = `${answered} respondidas`;
    }

    function updateMiniGrid() {
      const minis = document.querySelectorAll(".mini");

      minis.forEach((mini, index) => {
        const q = examQuestions[index];

        mini.classList.toggle("active", index === current);
        mini.classList.toggle("answered", answers[q.id] !== undefined);
      });
    }

    function calculateScore() {
      let correct = 0;

      examQuestions.forEach((q) => {
        if (answers[q.id] === q.answer) correct++;
      });

      const total = examQuestions.length;
      const percent = Math.round((correct / total) * 100);
      const score20 = ((correct / total) * 20).toFixed(2);

      return {
        correct,
        total,
        percent,
        score20
      };
    }

    function finishExam() {
      clearInterval(timerInterval);

      const result = calculateScore();
      const studentName = document.getElementById("studentName").value.trim();
      const endTime = new Date();

      document.getElementById("exam").classList.add("hidden");
      document.getElementById("results").classList.add("show");

      document.getElementById("scorePercent").textContent = `${result.percent}%`;
      document.getElementById("circle").style.setProperty("--deg", `${result.percent * 3.6}deg`);

      let message = "Necesita reforzamiento conceptual y práctica de problemas.";
      if (result.percent >= 85) message = "Excelente dominio conceptual y operativo.";
      else if (result.percent >= 70) message = "Buen desempeño, aunque debe afinar algunos fundamentos.";
      else if (result.percent >= 55) message = "Desempeño regular; requiere repasar definiciones y unidades.";

      document.getElementById("finalTitle").textContent = `${studentName}, nota: ${result.score20}/20`;
      document.getElementById("finalSummary").innerHTML = `
        Correctas: <strong>${result.correct}</strong> de <strong>${result.total}</strong>.
        Porcentaje: <strong>${result.percent}%</strong>. ${message}<br>
        Inicio: ${formatDate(startTime)} | Fin: ${formatDate(endTime)}
      `;

      buildReview();
    }

    function buildReview() {
      const review = document.getElementById("review");
      review.innerHTML = "";

      examQuestions.forEach((q, index) => {
        const selected = answers[q.id];
        const isCorrect = selected === q.answer;

        const item = document.createElement("div");
        item.className = "review-item";

        item.innerHTML = `
          <h3>${isCorrect ? "✅" : "❌"} ${index + 1}. ${q.question}</h3>
          <p><strong>Tu respuesta:</strong> ${selected === undefined ? "Sin responder" : letters[selected] + ". " + q.options[selected]}</p>
          <p><strong>Respuesta correcta:</strong> ${letters[q.answer]}. ${q.options[q.answer]}</p>
          <p><strong>Fundamento:</strong> ${q.explanation}</p>
        `;

        review.appendChild(item);
      });
    }

    function downloadReport() {
      const result = calculateScore();
      const studentName = document.getElementById("studentName").value.trim();

      const report = {
        estudiante: studentName,
        fecha: new Date().toISOString(),
        correctas: result.correct,
        total: result.total,
        porcentaje: result.percent,
        nota_vigesimal: result.score20,
        respuestas: examQuestions.map((q, index) => ({
          numero: index + 1,
          tema: q.topic,
          nivel: q.level,
          pregunta: q.question,
          respuesta_estudiante: answers[q.id] === undefined ? "Sin responder" : q.options[answers[q.id]],
          respuesta_correcta: q.options[q.answer],
          correcto: answers[q.id] === q.answer,
          explicacion: q.explanation
        }))
      };

      const blob = new Blob([JSON.stringify(report, null, 2)], {
        type: "application/json"
      });

      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");

      link.href = url;
      link.download = `reporte_${studentName.replaceAll(" ", "_").toLowerCase()}.json`;
      link.click();

      URL.revokeObjectURL(url);
    }

    function restartExam() {
      clearInterval(timerInterval);

      document.getElementById("setup").classList.remove("hidden");
      document.getElementById("exam").classList.add("hidden");
      document.getElementById("results").classList.remove("show");
    }

    function formatDate(date) {
      if (!date) return "---";

      return new Date(date).toLocaleString("es-PE", {
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit"
      });
    }
  </script>
</body>
</html>
