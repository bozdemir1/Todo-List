<template>
  <div id="app">
    <todos/>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {
  name: 'App',
  components: {
    Todos
  }
}
</script>

<style>
  html,
  body {
    margin: 0;
    padding: 0;
    background-image: url('http://orig00.deviantart.net/acb6/f/2013/303/7/6/gif___running_through_the_rain_by_turst67-d6scius.gif');
    background-size: cover;
    background-attachment: fixed;
  }

  button {
    margin: 0;
    padding: 0;
    border: 0;
    background: none;
    font-size: 100%;
    vertical-align: baseline;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    -webkit-appearance: none;
    appearance: none;
    -webkit-font-smoothing: antialiased;
    -moz-font-smoothing: antialiased;
    font-smoothing: antialiased;
  }
</style>
