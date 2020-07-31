<template>
  <div class="container">
    <h1>Simon Says</h1>
    <hr>
    <Simon/>
  </div>
</template>

<script>
import Simon from './components/Simon'
export default {
  name: 'App',
  components: { 
    Simon
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
body {
  font-family: 'Roboto', sans-serif;
  background: rgb(235, 235, 235);
}
.container {
  max-width: 800px;
  margin: 0 auto;
}
h1 {
  text-align: center;
}
</style> 
