<template >
<div>
  <div v-if="players">
      <div v-for="n in 10" :key="n">
          <Inning
            :in="n"
            v-bind:class="{ 'grid-blank': n==1, 'grid-inning': n>1}"
          />
      </div>
  </div>
  
  <div  v-for="player in players" :key="player.name">
    <!-- need to filter players TODO Remove v-if -->
    <div v-if="player.id<=9">
    <router-link to="/PlayerInfo">
          <Player 
              :id="player.id"
              :player="player"
              class="grid-player"
          />
    </router-link>
    <div v-for="scoreItem in scores[player.id-1]" :key="scoreItem.id">
      <router-link :to="{name: 'AtBatPage', params:{playerId:player.id-1,cellId:scoreItem.id}}">
          <AtBat
            :plays="scoreItem.plays"
            :hits="scoreItem.hits"
            :runs="scoreItem.runs"
            class="grid-item"
          />
      </router-link>
    </div>
   
  </div>
  </div>
</div>
</template>

<script>
import  { Data } from '../shared';
import Player from '../components/ScoreSheet/Player';
import AtBat from '../components/ScoreSheet/AtBat';
import Inning from '../components/ScoreSheet/Inning';

export default {
  name: 'ScoreSheet',
  components:  {
    Player,
    AtBat,
    Inning
  },
  props: {
    id: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      players: [],
      scores: [],
      Innings: [1,2,3,4,5,6,7,8,9]
    };
  },
  computed:{
    
  },
  async created() {
    await this.loadPlayers();
    await this.loadScores();
  },
  methods: {
    async loadScores() {
          this.scores = [];
          this.scores = await Data.getScores();
    },
    async loadPlayers() {
      this.player = [];
      // this.message = 'getting the Players, please be patient';
      this.players = await Data.getPlayers();
    }
  }
};
</script>
