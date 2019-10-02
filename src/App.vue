<template>
  <router-view></router-view>
</template>
<script>
import Todos from './components/Todos'
export default {
  name: 'app',
  components: {
    Todos
  }
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
