<template>
  <TablaPosiciones :positions="getPositions"></TablaPosiciones>
  <!-- <select v-model="activeGroup" name="select">
    <option v-for="group in groups" :value="group" :key="group">{{ group }}</option>
  </select>
  <div>{{ activeGroup }}</div> -->
  <div>
    <table>
      <tbody class="tbodyClass">
        <div class="styleDate">
          <ul v-for="fecha in uniqueNames" :key="fecha"> {{ formatDate(fecha) }} </ul>
        </div>
        <div class="styleTeams">
          <tr v-for="partido in partidos" :key="partido.id">
            <div class="styleDateMobile">
              <ul >{{ formatDate(partido.fecha) }} </ul>
            </div>
            <div class="ulclass">
              <div class="teamCenter"> {{ getTeamName(partido.equipoA) }} <input @change="hasMatchBeenPlayed($event,partido.id)" v-model="partido.golesEquipoA" class="inputClass" type="number"></div>
              <div class="teamCenter"> {{ getTeamName(partido.equipoB) }} <input @change="hasMatchBeenPlayed($event,partido.id)" v-model="partido.golesEquipoB" class="inputClass" type="number"></div>
            </div>
          </tr>
        </div>
      </tbody>
    </table>
  </div>
</template>

<script>

import axios from 'axios'
import TablaPosiciones from './TablaPosiciones.vue'

export default {
  name: 'PartidosJugados',
  components: {TablaPosiciones},
  data() {
    return {
      data: [],
      partidos: [],
      activeGroup: "C",
      groupTeams: [],
    }
  },
  mounted() {
    axios.get('https://especialess3.lanacion.com.ar/22/03/mundial2022-fixture/data/fechas.json')
      .then(response => {
        this.data = response.data.filter(dato => dato.grupo === this.activeGroup)
        this.getMatchs()
        console.log(this.fecha)
    axios.get('https://especialess3.lanacion.com.ar/22/03/mundial2022-fixture/data/diccEquipos.json')
      .then(response => {
        this.groupTeams = response.data.filter(dato => dato.grupo[0] === this.activeGroup)
        console.log(response.data)
        })
        console.log(this.data)
      })
  },
  computed: {
    uniqueNames() {
      return [...new Set(this.data.map(item => item.fecha))]
    },
    uniqueItems() {
      return [...new Set(this.data.map(item => item.fecha))]
    },
    getPositions(){
      const teams = []
      for (let team = 1; team < 5; team++) {
        teams.push("C"+team)
      }
      const positions = []
      teams.forEach(team => {
        const position = this.getTeamStats(team)
        positions.push(position)
      })
      return positions.sort(function (a, b) {
        if (a.points > b.points) {
          return -1;
        }
        if (a.points < b.points) {
          return 1;
        }
        // a must be equal to b
        return 0;
      });
    }
  },
  methods:{
    getTeamName(team) {
      let teamName = "" 
        switch (team) {
          case 'C1':
            teamName = 'Argentina' 
            break
          case 'C2':
            teamName = 'a.Saudita' 
            break
          case 'C3':
            teamName = 'Mexico' 
            break
          case 'C4':
            teamName = 'Polonia' 
            break
        } 
        return teamName
    },

    formatDate(date) {
      const dateObject = new Date(date);
      const weekdays = ['domingo', 'lunes', 'martes', 'miércoles', 'jueves', 'viernes', 'sábado'];
      const months = ['enero', 'febrero', 'marzo', 'abril', 'mayo', 'junio', 'julio', 'agosto', 'septiembre', 'octubre', 'noviembre', 'diciembre'];
      const weekday = weekdays[dateObject.getDay()];
      const month = months[dateObject.getMonth()];
      return `${weekday} ${dateObject.getDate()} de ${month}`;
    },
    getMatchs(){
      this.data.forEach(partido => {
        const result = {
            fecha:partido.fecha,
            id:partido.idpartido,
            equipoA:partido.equipoA,
            equipoB:partido.equipoB,
            golesEquipoA:"",
            golesEquipoB:"",
            jugado: false
        }
        this.partidos.push(result)
      })
    },
    hasMatchBeenPlayed(event,idpartido){

      const currentMatch = this.partidos.filter(partido => partido.id === idpartido)[0]

      if (event.target.value !== "") {
        currentMatch.jugado = true
      }      console.log(this.getPositions)
    },
    getTeamStats(team){
      let matches = 0
      let goalsFor = 0
      let goalsAgainst = 0
      let matchesWon = 0
      let matchesTied = 0
      let matchesLost = 0
      let goalsDifference = 0
      this.partidos.forEach(partido => {
        if ((partido.equipoA === team || partido.equipoB === team) && partido.jugado === true) { matches= matches+1 }
        if (partido.jugado){
          if (partido.equipoA === team){
            goalsFor += partido.golesEquipoA
            goalsAgainst += partido.golesEquipoB
            if (partido.golesEquipoA > partido.golesEquipoB){
              matchesWon= matchesWon+1
            }else if(partido.golesEquipoA < partido.golesEquipoB){
              matchesLost= matchesLost+1
            }else{
              matchesTied= matchesTied+1
            }
          }
          else if(partido.equipoB === team){
            goalsFor += partido.golesEquipoB
            goalsAgainst += partido.golesEquipoA
            if (partido.golesEquipoB > partido.golesEquipoA){
              matchesWon= matchesWon+1
            }else if(partido.golesEquipoB < partido.golesEquipoA){
              matchesLost= matchesLost+1
            }else{
              matchesTied= matchesTied+1
            }
          }
          goalsDifference = goalsFor - goalsAgainst
        }
      })
      const teamPoints= matchesWon*3+matchesTied
      return {
          team: this.getTeamName(team),
          points:teamPoints,
          matchesPlayed: matches,
          matchesWon:matchesWon,
          matchesTied:matchesTied,
          matchesLost:matchesLost,
          goalsFor:goalsFor,
          goalsAgainst:goalsAgainst,
          goalsDifference:goalsDifference,
        }
      
    },
    
  },
}

</script>

<style scoped>
.ulclass {
  font-family: 'suecananobold', sans-serif;
  border: 1px solid #ff6665;
  border-radius: 6px;
  background-color:#ff6665;
  height: 300px;
  width: 300px;
  justify-content: flex-start;
  color: white;
  font-size: 25px;
  text-transform: uppercase;
  margin-bottom: 10px;
}

    @media only screen and (max-width:1400px) {

  .ulclass {
    height: 70px;
    width: 370px;
    display: flex;
  }
}

td {
  float: left;
  width: 33.3333%;
}
.styleDate{
  column-count: 3;
  display: flex;
  justify-content: space-around;
  text-transform: uppercase;
  font-family: 'suecananobold', sans-serif;
  color: #2f2f2f88;
}
@media only screen and (max-width:1400px) {

  .styleDate {
    display: none;
    grid-template-columns: 1fr;
    grid-template-rows: auto;

  }
}

.styleDateMobile{

  column-count: 3;
  font-family: 'suecaslab', sans-serif;
  display: none;
  justify-content: space-around;
  text-transform: uppercase;
}
@media only screen and (max-width:1400px) {

  .styleDateMobile {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: auto;
    margin-left: -189px;
    font-family: 'suecananobold', sans-serif;
    color: #2f2f2f88;
  }
}

.inputClass{
  width: 45px;
  background-color: #f34241;
  border-radius: 6px;
  border: 1px solid #f34241;
  height: 37px;
  color: white;
  font-size: 30px;
  font-family: 'suecananobold', sans-serif;
  display: flex;
  justify-self: flex-end;

}
@media only screen and (max-width:1400px) {
  /* CSS rules for devices with a screen width of 600px or less */
  .inputClass {
    width: 29px;
  }
}
.styleTeams{
  column-count: 3;

}
@media only screen and (max-width:1400px) {
  /* CSS rules for devices with a screen width of 600px or less */
  .styleTeams {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: auto;
    margin-right: -20px;
  }
}
.teamCenter{
  align-items: center;
  margin-top: 20px;
  grid-template-columns: 1fr 1fr;
  display: grid;
  margin-left: 20px;
  justify-items: flex-start;
  margin-right: 20px;
  border-bottom: 1px solid white;
  padding-bottom: 12px;
}
@media only screen and (max-width:1400px) {
  /* CSS rules for devices with a screen width of 600px or less */
  .teamCenter {
    border-bottom: none;
    margin-left: 0px;
    font-size: 20px;
    margin-top: 13px;
    margin-left: 8px;
  }
}


</style>
