<template>
  <div id="app">
    <lucky-wheel></lucky-wheel>
  </div>
</template>

<script>
import LuckyWheel from './components/luckywheel'
export default {
  name: 'app',
  components: {
    LuckyWheel
  },

}
</script>
<style>

a {
    text-decoration: none;
}

button {
    overflow: visible;
    outline: none;
    border: none;
    background: none;
    list-style-type: none;
}
li {
    list-style: none;
}

html {
    height: 100%;
    font-family: sans-serif;
    -ms-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%
}

body {
    margin: 0 auto;
    /* max-width: 1320px; */
    min-width: 280px;
    font-family: 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif, 'Droid Sans Fallback';
    font-size: 16px;
    height: 100%;
    background: #fff;
}

div {
    box-sizing: border-box
}
</style>
