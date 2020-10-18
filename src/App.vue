<template>
  <div id="app">
    <h1>
      Cycles
      <br>
      {{cycles}}
    </h1>
    <Header/>
    <Info v-if="showInfo"/>
    <Main v-else @incrementCycle="cycles += 1" @resetCycle="cycles = 0"/>
    <div class="navs">
      <ButtonNav 
        @click="showInfo = !showInfo"
        :iconClass="showInfo ? 'mdi-arrow-left' : 'mdi-information-outline'"
      />
    </div>
  </div>
</template>

<script>
import Main from './components/Main'
import ButtonNav from './components/ButtonNav'
import Info from './components/Info'
import Header from './components/Header'
export default {
  name: 'App',
  components: {
    Main,
    ButtonNav,
    Info,
    Header
  },
  data(){
    return {
      cycles: 0,
      showInfo: false
    }
  }
}
</script>

<style>
html, body{
  min-height: 100vh;
  margin: 0;
  padding: 0;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-tap-highlight-color: rgba(0,0,0,0);
}
#app {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  min-height: 100vh;
  text-align: center;
  background: #009FFF;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to top, #ec2F4B, #009FFF);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to top, #00071f, #010823, #020a26, #030b2a, #040c2d, #101237, #1f1440, #2f1447, #511251, #730a54, #940050, #b10046) fixed;
}

.navs{
  padding: 20px;
}

button{
  border: none;
  background: rgba(0,0,0,0);
  color: white;
  cursor: pointer;
  outline: none;
}

button:hover{
  color: rgba(255, 255, 255, 0.712);
}
</style>
