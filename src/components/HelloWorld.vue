<template>
  <div class="hello">
    <!-- <h1>{{ msg }}</h1> -->
    <div id='MenuButtons'>
      <button v-on:click="getJSON">Load Files</button>
      <input v-model="playerName" type="text" placeholder="Send funds: pycoin address: 30819f300d06092a...." />
      <button v-on:click="getContent">Get Player Info</button>
      <button v-on:click="getOtherStuff">Get All Player Stats</button>
      <button v-on:click="profile">Get Player Shots</button>
      <button v-on:click="getPlayerStatsByPlayer">Player Stats Single</button>
      <button v-on:click="overAll">DashStats</button>

    </div>
          <ul id="baseStats">
            <li v-for="item in playerData.playerHeadlineStats" :key="item.id">
              Name: {{ item.playerName }} <br>
              ID: {{ item.playerId }} <br>
              Points Average: {{ item.pts }}  <br>
              Assists Average: {{ item.ast }}  <br>
              Rebound Average: {{ item.reb }}  <br>
            </li>
          </ul>
          <!-- <pre>{{dashStats}}</pre> -->
          <ul id="dashStatsDiv">
            <li v-for="item in dashStats.monthPlayerDashboard" :key="item.id">
              <!-- {{item}} -->
              Month: {{item.groupValue}} <br>
              Games Played: {{item.gp}} <br>
              Wins: {{item.w}} <br>
              Losses: {{item.l}} <br>
            </li>
          </ul>
          <ul id="fullStats">
            <li v-for="item in playerStats" :key="item.id">
              {{item}}
            </li>
          </ul>
          <p>{{playerData}}</p>
          <p>{{playerData.playerHeadlineStats}}</p>
          <h1 v-if="ok">Yes</h1>
          <h1 v-else>No</h1>
          <pre>{{ playerStats }}</pre>
          <p>{{playersLoaded}}</p>
          <pre>{{ nbaJson }}</pre>
          <pre>{{ playerShots }}</pre>
          <!-- <pre>{{ playerStats }}</pre> -->

  </div>
</template>

<script>
import NBA from 'nba'
// import { data, stats } from 'nba.js'
export default {
  name: 'HelloWorld',
  data () {
    return {
      playerName: 'Derrick Rose',
      msg: 'Welcome to Your Vue.js App',
      nbaJson: {stuff: 'gas'},
      playerData: {},
      playerShots: {},
      playerStats: {},
      dashStats: {},
      ok: false,
      playersLoaded: false
    }
  },
  methods: {
    async getContent () {
      let vm = this
      console.log('asynch test')
      await this.getJsonNew().then(function (result) { vm.playerData = result })
      console.log('asynch awaited')
      console.log()
      vm.ok = true
    },
    getJsonNew () {
      // const name = NBA.findPlayer('Derrick Rose');
      const name = NBA.findPlayer(this.playerName)
      return NBA.stats.playerInfo({ PlayerID: name.playerId })
    },
    async getOtherStuff () {
      let vm = this
      await this.getPlayerStats().then(function (result) { vm.nbaJson = result })
      vm.playersLoaded = true
    },
    getPlayerStats () {
      const rose = NBA.findPlayer('Derrick Rose')
      return NBA.stats.playerStats({ PlayerId: rose.playerId })
    },
    async profile () {
      console.log('profile shot')
      let vm = this
      await this.getPlayerProfile().then(function (result) { vm.playerShots = result })
      console.log('profile shots done')
    },
    getPlayerProfile () {
      const name = NBA.findPlayer(this.playerName)
      return NBA.stats.shots({ PlayerID: name.playerId })
    },
    async overAll () {
      console.log('Dash Stats shot')
      let vm = this
      await this.getoverAllDash().then(function (result) { vm.dashStats = result })
      console.log('Dash Stats done')
    },
    getoverAllDash () {
      const name = NBA.findPlayer(this.playerName)
      return NBA.stats.playerSplits({ PlayerID: name.playerId })
    },
    getPlayerStatsByPlayer () {
      let vm = this
      const name = NBA.findPlayer(this.playerName)
      console.log(name.playerId)
      let data = vm.nbaJson.leagueDashPlayerStats
      // console.log(vm.nbaJson.leagueDashPlayerStats)
      console.log(data)
      let playerIdToMatch = name.playerId

      // iterate over each element in the array
      for (let i = 0; i < data.length; i++) {
        // look for the entry with a matching `code` value
        if (data[i].playerId === playerIdToMatch) {
          // we found it
          console.log('found it')
          console.log(data[i])
          vm.playerStats = data[i]
          // obj[i].name is the matched result
        }
      }
    },
    getJSON (ev) {
      // let vm = this
      const rose = NBA.findPlayer('Derrick Rose')
      console.log('Rose Data; ' + JSON.stringify(rose))
      NBA.stats.playerInfo({ PlayerID: rose.playerId }).then(console.log)
      // console.log(teststats)
      // let testStuff = NBA.stats.playerInfo({ PlayerID: rose.playerId })
      // console.log(testStuff)
      // nba.stats.allPlayers()

      // NBA.stats.playerStats({ PlayerId: rose.playerId }).then(console.log)

      // working ones:
      // NBA.stats.shots({ PlayerID: rose.playerId }).then(console.log)
      // NBA.stats.playByPlay({ GameID: 1 }).then(console.log)
      // OverAll Player Dash
      NBA.stats.playerSplits({ PlayerID: rose.playerId }).then(console.log)

      // console.log(NBA.sportVu)
      // console.log(NBA.synergy)
      // console.log(NBA)
      // console.log('base:' + JSON.stringify(NBA))
      // let test = {}
      // NBA.stats.playerInfo({PlayerID: rose.playerId}).then(console.log).then(data => test = data)
      // console.log(test)
      // console.log(otherTest)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#baseStats{
  /* float: left; */
  margin: auto;
  width: 80%;
  padding: 5px;
  border: 1px solid gray;
}
#dashStatsDiv{
  margin: auto;
  margin-top: 5px;
  width: 80%;
  border: 1px solid orange;
  padding: 5px;
}
#MenuButtons{
  border: 1px solid gray;
}
</style>
