<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>
          <img src="@/assets/logo.svg" alt="Maya Logo" class="logo" @click="redirectToHome" />
          Maya
        </ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <div class="welcome-message">
        <h2 class="animate-text">MAYA</h2>
        <p>Ask MAYA, and get the data you need instantly. Whether it's retrieving business information or answering your queries, MAYA is here to assist.</p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
export default {
  name: 'WelcomePage',
  methods: {
    redirectToHome() {
      window.location.href = 'http://localhost:8100/Main/MAYA';
    },
  },
};
</script>

<style scoped>
/* Background */
ion-content {
  background: linear-gradient(to bottom right, #e0f7fa, #b3e5fc);
  overflow: hidden;
  position: relative;
}

/* Background Animation */
@keyframes backgroundAnim {
  0% { background-position: 0% 0%; }
  50% { background-position: 100% 100%; }
  100% { background-position: 0% 0%; }
}
ion-content::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(255,255,255,0.2), rgba(0,0,0,0.1));
  animation: backgroundAnim 10s ease infinite;
  pointer-events: none;
}

/* Welcome Message Styles */
.welcome-message {
  text-align: center;
  margin-top: 50px;
  animation: fadeIn 2s ease-in-out;
}

.welcome-message h2 {
  margin-bottom: 20px;
  font-size: 3em;
  color: #333;
  text-shadow: 2px 2px 5px rgba(0,0,0,0.3);
}

/* Description Text */
.welcome-message p {
  font-size: 1.2em;
  color: #555;
  margin: 0 20px;
  animation: fadeIn 3s ease-in-out;
  line-height: 1.6;
}

/* Text Animation */
@keyframes fadeIn {
  0% { opacity: 0; transform: translateY(-20px); }
  100% { opacity: 1; transform: translateY(0); }
}

.animate-text {
  animation: textAnim 3s ease-in-out infinite alternate;
}

@keyframes textAnim {
  0% { color: #0077b6; text-shadow: 0 0 10px rgba(0,119,182,0.6); }
  100% { color: #48cae4; text-shadow: 0 0 10px rgba(72,202,228,0.6); }
}

/* Logo styles with hover effect */
.logo {
  height: 30px;
  margin-right: 8px;
  vertical-align: middle;
  transition: transform 0.3s ease, filter 0.3s ease;
}

.logo:hover {
  transform: scale(1.1);
  filter: brightness(1.2);
}
</style>
