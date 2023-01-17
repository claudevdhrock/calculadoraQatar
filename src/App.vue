<template class="StyloFather">  
  <div class="StyloChilds">
    <PartidosJudados/>
  </div>
</template>

<script>
import PartidosJudados from './components/PartidosJudados.vue'


export default {
  name: 'App',
  components: {
    PartidosJudados,
  }
}
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
.StyloChilds{
  display: flex;
  justify-content: space-evenly;
}
@media only screen and (max-width: 1400px) {
  .StyloChilds {
    display: grid;
    justify-items: center;
  }
}

.StyloTabla{
  display: flex;
}


.StyloFather{
  display: flex;
}

</style>
