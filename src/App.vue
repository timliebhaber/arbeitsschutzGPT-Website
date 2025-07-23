<template>
  <PasswordProtected>
    <ModernChat msg="Welcome to Arbeitsschutz GPT" />
  </PasswordProtected>
</template>

<script>
import ModernChat from "./components/chat.vue";
import PasswordProtected from "./components/PasswordProtected.vue";

export default {
  name: "App",
  components: {
    ModernChat,
    PasswordProtected,
  },
};
</script>

<style>
html,
body,
#app {
  height: 100%;
  width: 100vw;
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #181c24 0%, #23272f 100%);
  color: #f3f6fa;
  font-family: "Segoe UI", "Arial", sans-serif;
  box-sizing: border-box;
  overflow: hidden;
}

#app {
  min-height: 100vh;
  min-width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  background: none;
  padding: 0;
}
</style>
