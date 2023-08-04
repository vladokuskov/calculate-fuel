<script setup>
import CalculateForm from './components/CalculateForm.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      <CalculateForm />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
.wrapper {
  padding: 0.5rem;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 2rem;
}
</style>
