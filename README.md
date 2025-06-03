<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Temporizador Circular con Frases</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    background: #f0f4f8;
    text-align: center;
  }
  h1 {
    margin-bottom: 0.5rem;
    color: #333;
  }
  #quote {
    margin: 1rem 0 2rem;
    font-style: italic;
    font-size: 1.25rem;
    color: #2e7d32;
    min-height: 40px;
    max-width: 320px;
  }
  #timer-container {
    position: relative;
    width: 200px;
    height: 200px;
  }
  svg {
    transform: rotate(-90deg);
    width: 200px;
    height: 200px;
  }
  circle {
    fill: none;
    stroke-width: 12;
    stroke-linecap: round;
  }
  #bg-circle {
    stroke: #ddd;
  }
  #progress-circle {
    stroke: red;
    stroke-dasharray: 565.48;
    stroke-dashoffset: 565.48;
    transition: stroke-dashoffset 1s linear;
  }
  #time-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 2rem;
    color: #444;
  }
  .buttons {
    margin-top: 2rem;
    display: flex;
    gap: 1rem;
  }
  button {
    padding: 12px 20px;
    font-size: 1rem;
    border: none;
    border-radius: 8px;
    background-color: #7e57c2;
    color: white;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  button:hover:enabled {
    background-color: #5e35b1;
  }
  button:disabled {
    background-color: #b39ddb;
    cursor: default;
  }
</style>
</head>
<body>

<h1>RespiraNet - Descanso Circular</h1>
<div id="quote"></div>

<div id="timer-container">
  <svg>
    <circle id="bg-circle" cx="100" cy="100" r="90"></circle>
    <circle id="progress-circle" cx="100" cy="100" r="90"></circle>
  </svg>
  <div id="time-text">5:00</div>
</div>

<div class="buttons">
  <button id="start-btn">Iniciar descanso (5 minutos)</button>
  <button id="motivation-btn">Mostrar frase motivadora</button>
</div>

<script>
  const positiveQuotes = [
    "Respira. Todo estará bien.",
    "Estás exactamente donde necesitas estar.",
    "Un momento de calma puede cambiar tu día.",
    "El descanso también es parte del progreso.",
    "Menos pantalla, más presencia."
  ];

  const motivationalQuotes = [
    "Deja tu teléfono.",
    "Haz tus deberes.",
    "Estudia.",
    "Haz lo que tienes que hacer."
  ];

  const progressCircle = document.getElementById('progress-circle');
  const timeText = document.getElementById('time-text');
  const quoteDiv = document.getElementById('quote');
  const startBtn = document.getElementById('start-btn');
  const motivationBtn = document.getElementById('motivation-btn');

  const radius = 90;
  const circumference = 2 * Math.PI * radius;
  progressCircle.style.strokeDasharray = circumference;
  progressCircle.style.strokeDashoffset = circumference;

  let duration = 5 * 60; // 5 minutos en segundos
  let elapsed = 0;
  let intervalId = null;

  function setProgress(percent) {
    const offset = circumference * (1 - percent);
    progressCircle.style.strokeDashoffset = offset;
  }

  function formatTime(seconds) {
    const min = Math.floor(seconds / 60);
    const sec = seconds % 60;
    return `${min}:${sec < 10 ? '0' : ''}${sec}`;
  }

  function updateTimer() {
    elapsed++;
    const progressPercent = elapsed / duration;
    setProgress(progressPercent);
    timeText.textContent = formatTime(duration - elapsed);

    if (elapsed >= duration) {
      clearInterval(intervalId);
      timeText.textContent = "¡Descanso terminado!";
      quoteDiv.textContent = "Gracias por darte este momento para ti.";
      startBtn.disabled = false;
    }
  }

  function startTimer() {
    elapsed = 0;
    setProgress(0);
    timeText.textContent = formatTime(duration);
    quoteDiv.textContent = positiveQuotes[Math.floor(Math.random() * positiveQuotes.length)];
    startBtn.disabled = true;

    intervalId = setInterval(updateTimer, 1000);
  }

  function showMotivationalQuote() {
    // Solo muestra una frase motivadora aleatoria sin afectar temporizador
    const quote = motivationalQuotes[Math.floor(Math.random() * motivationalQuotes.length)];
    quoteDiv.textContent = quote;
  }

  startBtn.addEventListener('click', startTimer);
  motivationBtn.addEventListener('click', showMotivationalQuote);
</script>

</body>
</html>
