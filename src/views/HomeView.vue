<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <body>
  <nav class="navMenu">
    <a><RouterLink to="/loan">Loan</RouterLink></a>
    <a><RouterLink to="about">About</RouterLink></a>
  </nav>
  </body>
  <RouterView></RouterView>
</template>
<style>
* {
  margin: 0;
  padding: 0;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}



.navMenu {
  position: absolute;
  top: 10%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  background: transparent;
}

.navMenu a {
  color: #000000;
  text-decoration: none;
  font-size: 1.2em;
  text-transform: uppercase;
  font-weight: 500;
  display: inline-block;
  width: 80px;
  -webkit-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  background: transparent;

}

.navMenu a:hover {
  color: #c63afd;
}
</style>
