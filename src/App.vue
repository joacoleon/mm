<template>
  <v-app>
    <v-main>
      <v-container>
        <p class="text-h2 text-center">Mixito Maker</p>
        <v-row class="mt-5" justify="center">
          <v-col v-for="(p, index) in sourcePlayers" :key="index" cols="auto">
            <v-card class="player-card text-center mx-1 mb-3 px-5 py-1"
              :class="[p.checked ? 'bg-blue-accent-3' : '', p.hasToPlay ? 'bg-orange-accent-1' : '']"
              @click="checkPlayer(p)">
              <b>{{ p.id }}</b>
            </v-card>
          </v-col>
        </v-row>

        <v-row class="text-center mb-4">
          <v-col lg="6" sm="12" cols="12">
            <v-tooltip text="Si son 11, se seleccionan 10 al azar. Si son mas de 12, se seleccionan 12 al azar."
              location="bottom">
              <template v-slot:activator="{ props }">
                <v-btn class="ma-2" variant="outlined" color="orange-accent-3" v-bind="props" :disabled="haveToPlay || playersWerePickedNormally || playersWerePicked"
                  @click="loadRandom()">Cargar 10-12</v-btn>
              </template>
            </v-tooltip>
          </v-col>
          <v-col lg="6" sm="12" cols="12">
            <v-tooltip text="1. Usar cuando hay jugadores que tienen que jugar si o si." location="bottom">
              <template v-slot:activator="{ props }">
                <v-btn class="ma-2" variant="outlined" color="red-accent-3" v-bind="props"
                  :disabled="playersWerePickedNormally || playersWerePicked" @click="loadHaveToPlay()">
                  1. No jugaron
                </v-btn>
              </template>
            </v-tooltip>
            <v-tooltip text="2. Usar luego de haber seleccionado los que tienen que jugar si o si." location="bottom">
              <template v-slot:activator="{ props }">
                <v-btn class="ma-2" variant="outlined" color="red-accent-3" v-bind="props"
                  :disabled="playersWerePickedNormally || playersWerePicked" @click="loadRest()">
                  2. Cargar resto
                </v-btn>
              </template>
            </v-tooltip>
          </v-col>
        </v-row>

        <v-divider v-if="playersWerePicked"></v-divider>

        <div v-if="onlinePlayersDisplay.length" class="text-button mt-4 mb-1 text-center text-green">JUEGAN:</div>
        <v-row justify="center">
          <v-col v-for="(o, index) in onlinePlayersDisplay" :key="index" cols="auto">
            <v-card class="text-center px-5 bg-green">
              <b>{{ o.id }}</b>
            </v-card>
          </v-col>
        </v-row>

        <div v-if="haveToPlayPlayers.length" class="text-button mt-5 mb-1 text-center text-orange">TIENEN QUE JUGAR:
        </div>
        <v-row justify="center">
          <v-col v-for="(h, index) in haveToPlayPlayers" :key="index" cols="auto">
            <v-card class="text-center px-5 bg-orange">
              <b>{{ h.id }}</b>
            </v-card>
          </v-col>
        </v-row>

        <div v-if="mimirPlayers.length" class="text-button mt-6 mb-1 text-center text-red">SPECT:</div>
        <v-row justify="center">
          <v-col v-for="(m, index) in mimirPlayers" :key="index" cols="auto">
            <v-card class="text-center px-5 bg-red">
              <b>{{ m.id }}</b>
            </v-card>
          </v-col>
        </v-row>

        <v-row class="mt-9 mb-4 text-center" v-if="playersWerePicked">
          <v-col class="rounded-lg ml-2 mr-2">
            <v-btn x-large class="ma-2" variant="outlined" color="indigo-accent-3" @click="sortTeams()" append-icon="mdi-cached">
              ARMAR EQUIPOS
            </v-btn>
          </v-col>
        </v-row>

        <v-divider v-if="teamsWereSorted"></v-divider>

        <v-row v-if="teamsWereSorted" class="mt-4">
          <v-col>
            <v-card variant="tonal" class="pa-3 pt-4 text-center bg-indigo">
              <v-badge :content="sumA" floating color="indigo-lighten-3">
                <span justify="center" class="display-1 font-weight-bold">CT</span>
              </v-badge>
              <v-row>
                <v-col v-for="(i, index) in teamA" :key="index">
                  <span class="my-2 font-weight-bold">{{ i.id }}</span>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
          <v-col>
            <v-card variant="tonal" class="pa-3 pt-4 text-center bg-red">
              <v-badge :content="sumA" floating color="red-lighten-3">
                <span justify="center" class="display-1 font-weight-bold">TT</span>
              </v-badge>
              <v-row>
                <v-col v-for="(i, index) in teamB" :key="index">
                  <span class="my-2 font-weight-bold">{{ i.id }}</span>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>

      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from 'vue';

let sourcePlayers = ref([
  { id: "1M", level: 8, checked: false, audioId: null },
  { id: "Abusa", level: 7, checked: false, audioId: null },
  { id: "AFIP", level: 9, checked: false, audioId: null },
  { id: "Bichi", level: 6, checked: false, audioId: null },
  { id: "Bot", level: 6, checked: false, audioId: null },
  { id: "brusLEE", level: 9, checked: false, audioId: null },
  { id: "El Chad", level: 6, checked: false, audioId: null },
  { id: "Equi", level: 7, checked: false, audioId: null },
  { id: "FILI", level: 7, checked: false, audioId: null },
  { id: "Ficha", level: 7, checked: false, audioId: null },
  { id: "Fisi", level: 7, checked: false, audioId: null },
  { id: "FR1D4", level: 6, checked: false, audioId: null },
  { id: "jp", level: 5, checked: false, audioId: null },
  { id: "Legi", level: 7, checked: false, audioId: null },
  { id: "Leo", level: 7, checked: false, audioId: null },
  { id: "Nawi", level: 9, checked: false, audioId: null },
  { id: "Pela", level: 8, checked: false, audioId: null },
  { id: "pulp0", level: 5, checked: false, audioId: null },
  { id: "Rou", level: 5, checked: false, audioId: null },
  { id: "Rules", level: 6, checked: false, audioId: null },
  { id: "Sakura", level: 6, checked: false, audioId: null },
  { id: "Samarosky", level: 6, checked: false, audioId: null },
  { id: "vazko", level: 9, checked: false, audioId: null },
  { id: "zocho", level: 9, checked: false, audioId: null }
]);

const onlinePlayers = ref([]);
const mimirPlayers = ref([]);
const onlinePlayersDisplay = ref([]);

const haveToPlay = ref(false);
const haveToPlayPlayers = ref([]);

const teamA = ref([]);
const teamB = ref([]);
const sumA = ref();
const sumB = ref();

const playersWerePicked = ref(false);
const playersWerePickedNormally = ref(false);
const teamsWereSorted = ref(false);


const checkPlayer = (p) => {
  p.checked = !p.checked;
}

const checkPlayersLevels = () => {
  onlinePlayers.value.sort((a, b) => b.level - a.level);
}

const shuffle = (array) => { //Durstenfeld shuffle
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}

const loadRandom = () => {
  const checkedPlayers = sourcePlayers.value.filter((p) => p.checked);
  onlinePlayers.value = [...checkedPlayers];
  shuffle(onlinePlayers.value);

  const playerCount = onlinePlayers.value.length;
  if (playerCount === 11) {
    mimirPlayers.value = onlinePlayers.value.splice(0, playerCount - 10);
  } else if (playerCount > 12) {
    mimirPlayers.value = onlinePlayers.value.splice(0, playerCount - 12);
  } else {
    mimirPlayers.value = [];
  }

  checkPlayersLevels();
  onlinePlayersDisplay.value = [...onlinePlayers.value];
  playersWerePicked.value = true;
  playersWerePickedNormally.value = true;
};

const loadHaveToPlay = () => {
  haveToPlay.value = true;
  haveToPlayPlayers.value = sourcePlayers.value.filter((p) => p.checked || p.hasToPlay);
  sourcePlayers.value.forEach((p) => {
    if (p.checked) {
      p.checked = false;
      p.hasToPlay = true;
    }
  });
};

const loadRest = () => {
  const checkedPlayers = sourcePlayers.value.filter((p) => p.checked);
  onlinePlayers.value = [...checkedPlayers];
  const totalPlayers = onlinePlayers.value.length + haveToPlayPlayers.value.length;
  shuffle(onlinePlayers.value);

  if (totalPlayers === 11) {
    mimirPlayers.value = onlinePlayers.value.splice(0, totalPlayers - 10);
  } else if (totalPlayers > 12) {
    mimirPlayers.value = onlinePlayers.value.splice(0, totalPlayers - 12);
  } else {
    mimirPlayers.value = [];
  }

  onlinePlayers.value = [...onlinePlayers.value, ...haveToPlayPlayers.value];
  haveToPlayPlayers.value = [];
  checkPlayersLevels();
  onlinePlayersDisplay.value = [...onlinePlayers.value];
  playersWerePicked.value = true;
};

const sortTeams = () => {
  teamA.value = [];
  teamB.value = [];

  const players = [...onlinePlayers.value];
  const [first, second, ...rest] = players;

  if (Math.random() < 0.5) {
    teamA.value.push(first);
    teamB.value.push(second);
  } else {
    teamB.value.push(first);
    teamA.value.push(second);
  }

  rest.forEach((p) => {
    const sumTeamA = teamA.value.reduce((a, b) => a + b.level, 0);
    const sumTeamB = teamB.value.reduce((a, b) => a + b.level, 0);
    (sumTeamA <= sumTeamB ? teamA.value : teamB.value).push(p);
  });

  if (teamA.value.length !== teamB.value.length) {
    const longer = teamA.value.length > teamB.value.length ? teamA : teamB;
    const shorter = teamA.value.length > teamB.value.length ? teamB : teamA;
    shorter.value.push(...longer.value.splice(-1, 1));
  }

  sumA.value = teamA.value.reduce((a, b) => a + b.level, 0);
  sumB.value = teamB.value.reduce((a, b) => a + b.level, 0);
  teamsWereSorted.value = true;
};

const reset = () => {
  teamA = [];
  teamB = [];
  onlinePlayersDisplay = [];
  onlinePlayers = [];
  haveToPlayPlayers = [];
  mimirPlayers = [];
  sumA = null;
  sumB = null;
  teamsWereSorted = false;
  playersWerePicked = false;
  playersWerePickedNormally = false;
  haveToPlay = false;
  sourcePlayers.forEach((el) => el.checked = false);
  sourcePlayers.forEach((el) => el.hasToPlay = false);
}

const copyToClipboard = () => {
  var text = document.createElement("textarea");
  document.body.appendChild(text);
  let teamAText = [];
  let teamBText = [];
  teamA.forEach((el) => {
    teamAText.push(" " + el.id);
  });
  teamB.forEach((el) => {
    teamBText.push(" " + el.id);
  });
  text.value = "CT: " + teamAText + " | TT: " + teamBText;
  text.select();
  document.execCommand("copy");
  document.body.removeChild(text);
}

</script>
<style scoped>
.player-card:hover {
  opacity: 70%;
  cursor: pointer;
}
</style>
