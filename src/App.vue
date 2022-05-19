<template>
  <main>
    <Advice />
  </main>
  <footer>
    Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
    Coded by <a target="_blank" href="https://github.com/dombrga">Dom B</a>.
  </footer>
  
</template>

<script setup>
  import Advice from './views/Advice.vue'

</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&display=swap');

$mainBgColor: hsl(218, 23%, 16%);
$mainFont: 'Manrope', sans-serif;

* { 
  box-sizing: border-box; 
  text-decoration: none;
}

body {
  margin: 0;
  background-color: $mainBgColor;
  font-family: $mainFont;
  // padding: 6em 1em 1em;
}

#app {
  padding: 6em 1em 1em;
  // padding: auto;
}

main {
  // margin-bottom: 8em;
}

footer {
  color: hsl(220, 5%, 51%);
  font-size: .7rem;

  width: 100%;
  text-align: center;
  position: absolute;
  right: 0; left: 0;
  bottom: 5em;
  // top: 0

  > a {
    color: hsl(193, 33%, 52%);
    
    &:hover {
      text-decoration: underline;
    }
  }
}

</style>
