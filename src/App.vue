<template>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
  <div class="container">
    <!-- Partículas de fondo -->
    <div
      v-for="i in 35"
      :key="i"
      class="particle"
      :style="{
        left: Math.random() * 100 + '%',
        top: Math.random() * 100 + '%',
        animationDelay: Math.random() * 5 + 's',
        animationDuration: Math.random() * 15 + 10 + 's',
      }"
    >
      <component 
        :is="particleIcons[Math.floor(Math.random() * particleIcons.length)]" 
        :size="Math.random() * 12 + 10" 
        :stroke-width="1.5" 
      />
    </div>

    <!-- Contenido principal -->
    <div v-if="!acepto" class="content">
      <div class="card-glass">
        <div class="icon-header">
          <Heart :size="60" class="main-icon" :stroke-width="1.5" fill="currentColor" />
        </div>
        
        <h1><strong>¿ALISON CARRION QUIERES SER MI VALENTINE PARA TODA LA VIDA?</strong></h1>

        <div v-if="intentos > 0" class="attempt-counter">
          <AlertCircle :size="16" :stroke-width="2" />
          <span>Intentos: {{ intentos }}</span>
        </div>

        <div class="buttons">
          <button class="yes" @click="aceptar">
            <Heart :size="18" fill="currentColor" class="btn-icon" />
            <span>ACEPTO</span>
            <Sparkles :size="16" class="btn-icon" />
          </button>

          <button
            class="no"
            :class="{ moving: intentos > 0 }"
            @mouseover="moverBoton"
            @click="moverBoton"
            :style="intentos > 0 ? {
              top: noTop + 'px',
              left: noLeft + 'px',
              transform: `scale(${Math.max(0.6, 1 - intentos * 0.04)})`,
            } : {}"
          >
            <X :size="16" :stroke-width="2" class="btn-icon-no" />
            <span>{{ noTextos[Math.min(intentos, noTextos.length - 1)] }}</span>
          </button>
        </div>
      </div>
    </div>

    <!-- Mensaje de éxito -->
    <div v-else class="success">
      <div class="success-icon-container">
        <HeartHandshake :size="70" class="success-icon" :stroke-width="1.5" />
      </div>
      
      <h1>OFICIALMENTE ERES MI VALENTINE Y MI TODO</h1>
      <p class="subtitle">¡TE AMO TANTO!</p>

      <div class="love-letter">
        <p>Insisto, te amo tanto. Estos meses sé que no han sido los mejores, pero siempre hemos tratado de seguir adelante. Te agradezco por todo, por ser una mujer fuerte. Me alegra decir que eres mi novia, eres solo mía, me hace muy feliz. Y aunque pienses que no te amo, ten presente que no es necesario decirlo, sino el simple hecho de escribirte o llamarte apenas me despierto es porque siempre te tengo presente. En serio quiero cumplir todos nuestros sueños.</p>
        <p>Quiero que sepas que puedes contar conmigo, van a mejorar las cosas. Sabes, quisiera cumplir el sueño de que nos digan que somos doctores y poder ayudar a nuestros padres como siempre hemos querido. Siempre estaré aquí para ti, no estás sola, eres mi vida.</p>
      </div>

      <div class="hearts">
        <Heart :size="40" class="heart-float" :stroke-width="1.5" fill="currentColor" />
        <Sparkles :size="35" class="heart-float" :stroke-width="1.5" />
        <Star :size="38" class="heart-float" :stroke-width="1.5" fill="currentColor" />
      </div>
    </div>

    <!-- Confeti cuando acepta -->
    <div v-if="acepto">
      <div
        v-for="i in 80"
        :key="'conf' + i"
        class="confetti"
        :style="{
          left: Math.random() * 100 + '%',
          top: '-10px',
          background: ['#ff4d6d', '#ff758f', '#ffa8c5', '#ffccd5', '#fff', '#ffd700'][
            Math.floor(Math.random() * 6)
          ],
          animationDelay: Math.random() * 3 + 's',
          animationDuration: Math.random() * 3 + 2 + 's',
        }"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { 
  Heart, 
  Sparkles, 
  Star, 
  HeartHandshake,
  Sparkle,
  CircleDot,
  AlertCircle,
  X
} from 'lucide-vue-next';

const acepto = ref(false);
const noTop = ref(0);
const noLeft = ref(0);
const intentos = ref(0);

const particleIcons = [Heart, Sparkles, Star, Sparkle, CircleDot];

const noTextos = [
  'No',
  '¿Seguro?',
  'Piénsalo',
  'Dale',
  'Por favor',
  'Venga',
  'Anda',
  '¡Vamos!',
  '¡Sí!',
  'ACEPTO',
];

function aceptar() {
  acepto.value = true;
}

function moverBoton() {
  intentos.value++;

  const containerWidth = 500;
  const containerHeight = 180;

  noTop.value = Math.random() * containerHeight - containerHeight / 2;
  noLeft.value = Math.random() * containerWidth - containerWidth / 2;

  if (navigator.vibrate) {
    navigator.vibrate(100);
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: radial-gradient(ellipse at top, #3a1c3f, #1a1625),
              radial-gradient(ellipse at bottom, #2a1435, #0d0815);
  overflow: hidden;
  font-family: 'Poppins', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

/* Partículas flotantes de fondo */
.particle {
  position: absolute;
  pointer-events: none;
  animation: float 20s infinite ease-in-out;
  color: rgba(255, 182, 193, 0.25);
  opacity: 0.5;
}

@keyframes float {
  0%, 100% {
    transform: translate(0, 0) rotate(0deg);
    opacity: 0.2;
  }
  25% {
    opacity: 0.5;
  }
  50% {
    transform: translate(50px, -80px) rotate(180deg);
    opacity: 0.7;
  }
  75% {
    opacity: 0.3;
  }
}

/* Contenido principal */
.content {
  text-align: center;
  z-index: 10;
  animation: fadeIn 0.8s ease;
  width: 100%;
  max-width: 650px;
  padding: 30px;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Card con glassmorphism */
.card-glass {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border-radius: 30px;
  padding: 60px 50px 70px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3),
              inset 0 1px 0 rgba(255, 255, 255, 0.1);
  position: relative;
  max-width: 600px;
  margin: 0 auto;
}

.icon-header {
  margin-bottom: 35px;
  animation: iconFloat 3s ease-in-out infinite;
}

.main-icon {
  color: #ff6b9d;
  filter: drop-shadow(0 0 25px rgba(255, 107, 157, 0.6))
          drop-shadow(0 0 50px rgba(255, 107, 157, 0.3));
}

@keyframes iconFloat {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-15px) rotate(5deg);
  }
}

h1 {
  font-size: 1.8em;
  color: #fff;
  margin-bottom: 50px;
  text-shadow: 0 0 30px rgba(255, 107, 157, 0.4),
               0 2px 6px rgba(0, 0, 0, 0.3);
  font-weight: 400;
  letter-spacing: 0.5px;
  line-height: 1.4;
}

.buttons {
  position: relative;
  height: 180px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin-top: 30px;
}

button {
  padding: 0 40px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  height: 58px;
  min-width: 180px;
  backdrop-filter: blur(10px);
}

.btn-icon, .btn-icon-no {
  transition: transform 0.3s ease;
  flex-shrink: 0;
}

.yes {
  background: rgba(255, 107, 157, 0.3);
  border: 2px solid rgba(255, 107, 157, 0.6);
  color: #fff;
  box-shadow: 0 0 25px rgba(255, 107, 157, 0.4),
              inset 0 0 15px rgba(255, 107, 157, 0.1);
  animation: pulse 2s ease-in-out infinite;
}

.yes:hover {
  background: rgba(255, 107, 157, 0.5);
  border-color: rgba(255, 107, 157, 0.9);
  box-shadow: 0 0 40px rgba(255, 107, 157, 0.7),
              inset 0 0 25px rgba(255, 107, 157, 0.2);
  transform: scale(1.05);
}

.yes:hover .btn-icon {
  animation: heartPulse 0.6s ease infinite;
}

@keyframes heartPulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.3); }
}

@keyframes pulse {
  0%, 100% {
    box-shadow: 0 0 25px rgba(255, 107, 157, 0.4),
                inset 0 0 15px rgba(255, 107, 157, 0.1);
  }
  50% {
    box-shadow: 0 0 40px rgba(255, 107, 157, 0.6),
                inset 0 0 25px rgba(255, 107, 157, 0.2);
  }
}

.no {
  background: rgba(100, 100, 100, 0.15);
  border: 2px solid rgba(150, 150, 150, 0.25);
  color: rgba(255, 255, 255, 0.5);
  transition: all 0.2s ease !important;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2),
              inset 0 0 8px rgba(100, 100, 100, 0.1);
}

.no.moving {
  position: absolute !important;
}

.no:hover {
  background: rgba(100, 100, 100, 0.25);
}

/* Mensaje de éxito */
.success {
  color: white;
  text-align: center;
  animation: successZoom 1s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  z-index: 10;
  max-width: 700px;
  padding: 20px 30px 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

@keyframes successZoom {
  0% {
    transform: scale(0) rotate(-180deg);
    opacity: 0;
  }
  70% {
    transform: scale(1.15) rotate(10deg);
  }
  100% {
    transform: scale(1) rotate(0deg);
    opacity: 1;
  }
}

.success-icon-container {
  margin-bottom: 15px;
}

.success-icon {
  color: #ff6b9d;
  filter: drop-shadow(0 0 35px rgba(255, 107, 157, 0.8))
          drop-shadow(0 0 70px rgba(255, 107, 157, 0.5));
  animation: successPulse 2s ease-in-out infinite;
}

@keyframes successPulse {
  0%, 100% { 
    transform: scale(1) rotate(0deg);
  }
  50% { 
    transform: scale(1.1) rotate(5deg);
  }
}

.success h1 {
  font-size: 2.5em;
  color: white;
  text-shadow: 0 0 50px rgba(255, 107, 157, 0.8),
               0 4px 10px rgba(0, 0, 0, 0.5);
  margin-bottom: 10px;
  font-weight: 700;
  animation: rainbow 4s linear infinite;
}

@keyframes rainbow {
  0% { filter: hue-rotate(0deg); }
  100% { filter: hue-rotate(360deg); }
}

.success .subtitle {
  font-size: 1.05em;
  margin-bottom: 25px;
  text-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
  font-weight: 400;
  color: rgba(255, 255, 255, 0.85);
}

/* Carta de amor */
.love-letter {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(15px);
  border-radius: 20px;
  padding: 25px 30px;
  margin: 0 auto 25px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2),
              inset 0 1px 0 rgba(255, 255, 255, 0.1);
  max-width: 600px;
  text-align: center;
}

.love-letter p {
  font-size: 0.92em;
  line-height: 1.65;
  margin-bottom: 15px;
  color: rgba(255, 255, 255, 0.95);
  font-weight: 300;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.love-letter p:last-child {
  margin-bottom: 0;
}

.hearts {
  display: flex;
  justify-content: center;
  gap: 35px;
  color: #ff6b9d;
  filter: drop-shadow(0 0 25px rgba(255, 107, 157, 0.6));
}

.heart-float {
  animation: floatIcon 2.5s ease-in-out infinite;
}

.heart-float:nth-child(1) { animation-delay: 0s; }
.heart-float:nth-child(2) { animation-delay: 0.3s; }
.heart-float:nth-child(3) { animation-delay: 0.6s; }

@keyframes floatIcon {
  0%, 100% {
    transform: translateY(0) rotate(0deg) scale(1);
  }
  50% {
    transform: translateY(-20px) rotate(10deg) scale(1.1);
  }
}

/* Confeti */
.confetti {
  position: absolute;
  width: 12px;
  height: 12px;
  animation: confettiFall 4s linear infinite;
  pointer-events: none;
  border-radius: 50%;
  box-shadow: 0 0 8px currentColor;
}

@keyframes confettiFall {
  to {
    transform: translateY(120vh) rotate(720deg);
    opacity: 0;
  }
}

/* Contador de intentos */
.attempt-counter {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(255, 107, 157, 0.15);
  backdrop-filter: blur(10px);
  padding: 10px 18px;
  border-radius: 50px;
  font-weight: 600;
  color: #fff;
  border: 1px solid rgba(255, 107, 157, 0.3);
  box-shadow: 0 0 20px rgba(255, 107, 157, 0.25),
              inset 0 0 15px rgba(255, 107, 157, 0.1);
  animation: counterFade 0.5s ease;
  font-size: 13px;
  display: flex;
  align-items: center;
  gap: 8px;
}

@keyframes counterFade {
  from {
    opacity: 0;
    transform: translateX(15px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* Responsive */
@media (max-width: 768px) {
  .card-glass {
    padding: 50px 35px 60px;
    max-width: 90%;
  }

  h1 {
    font-size: 1.4em;
    margin-bottom: 40px;
  }
  
  .main-icon {
    width: 50px;
    height: 50px;
  }
  
  button {
    min-width: 150px;
    height: 54px;
    font-size: 15px;
    padding: 0 32px;
  }
  
  .buttons {
    gap: 30px;
    height: 160px;
  }
  
  .success {
    padding: 20px;
  }

  .success h1 {
    font-size: 2.2em;
  }
  
  .success .subtitle {
    font-size: 1em;
    margin-bottom: 25px;
  }

  .love-letter {
    padding: 25px 28px;
    margin-bottom: 30px;
  }

  .love-letter p {
    font-size: 0.9em;
    line-height: 1.6;
    margin-bottom: 15px;
  }
  
  .hearts {
    gap: 25px;
  }

  .heart-float {
    width: 35px;
    height: 35px;
  }
  
  .attempt-counter {
    top: 15px;
    right: 15px;
    padding: 8px 16px;
    font-size: 12px;
  }
}

@media (max-width: 480px) {
  .content {
    padding: 20px;
  }

  .card-glass {
    padding: 40px 25px 50px;
  }

  h1 {
    font-size: 1.2em;
    margin-bottom: 35px;
  }

  .buttons {
    gap: 25px;
    height: 140px;
  }

  button {
    min-width: 130px;
    height: 50px;
    font-size: 14px;
    padding: 0 28px;
    gap: 10px;
  }

  .success h1 {
    font-size: 1.8em;
  }

  .success .subtitle {
    font-size: 0.95em;
  }

  .love-letter {
    padding: 20px 22px;
    margin-bottom: 25px;
  }

  .love-letter p {
    font-size: 0.85em;
    margin-bottom: 12px;
  }
}
</style>