<template>
  <div >
    <img alt="Egypt logo" :style="'width: 200px; display: flex; justify-content: flex-start;'" src="./assets/egypt-express-logo.png">
    <Suspense>
      <StartPage/>
    </Suspense>
  </div>
</template>

<script>
import StartPage from './components/StartPage.vue';


export default {
  name: 'App',
  components: {
    StartPage
},
  
}
</script>

<style lang="scss">
#app {
  display: flex;
    flex-direction: column;
    margin: 10vh 8vw 10vh 8vw;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
