<template>
  <div class="classTable">
    <table>
      <tr >
        <td class="classSemiBold">POSICIONES</td>
        <td class="classSemiBold">PTS</td>
        <td class="classSemiBold">PJ</td>
        <td class="classSemiBold">G</td>
        <td class="classSemiBold">E</td>
        <td class="classSemiBold">P</td>
        <td class="classSemiBold">GF</td>
        <td class="classSemiBold">GC</td>
        <td class="classSemiBold">DG</td> 
     </tr>
     <tbody>
        <tr v-for="position in positions" :key="position.team" >
          <td class="classSemiBold">{{ position.team }}</td>
          <td class="classSemiBold">{{ position.points }}</td>
          <td class="classSemiBold">{{ position.matchesPlayed }}</td>
          <td class="classSemiBold">{{ position.matchesWon }}</td>
          <td class="classSemiBold">{{ position.matchesTied }}</td>
          <td class="classSemiBold">{{ position.matchesLost }}</td>
          <td class="classSemiBold">{{ position.goalsFor }}</td>
          <td class="classSemiBold">{{ position.goalsAgainst }}</td>
          <td class="classSemiBold">{{ position.goalsDifference }}</td>
        </tr>
     </tbody>
    </table>
  </div>
</template>

<script>
// import axios from 'axios'

export default {
  name: 'TablaPosiciones',
  props: ["positions"],
  // data() {
  //   return {
  //     equipos: []
  //   }
  // },
  // mounted() {
  //   axios.get('https://especialess3.lanacion.com.ar/22/03/mundial2022-fixture/data/diccEquipos.json')
  //     .then(response => {
  //       this.equipos = response.data.filter(dato => dato.grupo[0] === "C")
  //     })
  //     console.log(this.positions)
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.classSemiBold{
  color: #ff6665;
  font-family: 'suecananomedium', sans-serif;
  font-size: 20px;
  padding: 5px;
}
.classBold{
  color: #ff6665;
  font-family: 'suecananobold', sans-serif;
  border-bottom: 0.5px solid #ff6665;
}
.classLight{
  color: #ff6665;
  font-family: 'suecaslab', sans-serif;
}
tr{
  color: #ff6665;
  font-family: 'suecananomedium', sans-serif;
  font-size: 20px;
  padding: 5px;
  border-bottom: 1px solid #ff6665;

  border-collapse: collapse;
}
td{
  color: #ff6665;
  font-family: 'suecananomedium', sans-serif;
  font-size: 20px;
  padding: 5px;
  border-right: 1px solid #ff6665;
  border-collapse: collapse;
  padding: 12px;
  width: 25px;
}
table{
  border-bottom: 0.5px solid #ff6665;
  border-right: 0.5px solid #ff6665;
  border-collapse: collapse;
  margin: 10px;
}
.classTable{
  display:flex;
}
@media only screen and (max-width: 1400px) {
  /* CSS rules for devices with a screen width of 600px or less */
  .classTable {
    display: grid;
    width: 24rem;
    grid-template-columns: 1fr;
    grid-template-rows: auto;
  }
}

</style>

