<template>
  <div class="container">
  <Donate/>
  </div>
</template>

<script setup lang="ts">
import Donate from './components/Donate.vue';

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  white-space: pre-wrap;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #51565c;
  margin-top: 60px;
}
button { 
  border-radius: 4px; 
  border: 1px solid lightgrey;
  border-bottom: 2px solid lightgrey; 
  color: #51565c;
  background-color: transparent;
  cursor: pointer;
  font-weight: bold;
  padding: .8em 1em;
}
button:active {
  transform: scale(0.98);
}
button:disabled,
button[disabled] {
  background-color: rgb(72, 67, 67);
}
.container {
  max-width: 400px;
}
</style>
