<script setup>
import Sidebar from './components/Sidebar.vue';
import Navbar from './components/Navbar.vue';
import Main from './components/Main.vue';
</script>

<template>
  <div class="layout">
    <aside class="sidebar">
      <Sidebar />
    </aside>
    <div class="right-section">
      <header class="navbar">
        <Navbar />
      </header>
      <main class="main-content">
        <Main />
      </main>
    </div>
  </div>
</template>

<style scoped>
.layout {
  display: flex;
  height: 100vh;
}


.right-section {
  display: flex;
  flex-direction: column;
  flex: 1;
  /* Take remaining horizontal space */
}

.main-content {
  flex: 1;
  /* Take remaining vertical space */
  background-color: #ecf0f1;
  padding: 1rem;
  overflow-y: auto;
}
</style>
