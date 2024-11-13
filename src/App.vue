<template>
   <PasswordProtected>
  <chat msg="Welcome to Your Vue.js App"/>
   </PasswordProtected>
</template>

<script>
import chat from './components/chat.vue'
import PasswordProtected from './components/PasswordProtected.vue';

export default {
  name: 'App',
  components: {
    chat,
    PasswordProtected
  }
}
</script>

<style>
/* Globale dunkle Hintergrundfarbe */
#app {
  background-color: #121212;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  background-color: #121212; /* Dunkler Hintergrund */
  color: #e0e0e0; /* Helle Textfarbe */
  font-family: Arial, sans-serif;
}
</style>
