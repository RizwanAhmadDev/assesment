<template>
  <div id="app">
    <EmailForm />
  </div>
</template>

<script>
import EmailForm from './components/EmailForm.vue';

export default {
  name: 'App',
  components: {
    EmailForm,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
