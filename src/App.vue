<template>
  <div class="main-content">
    <div>
    <p>Player Name</p>
    <input class="name-input" type="`text`" v-model="input" @keydown.enter="addInput()">

    <!-- <div>
      <button v-for="item in all" @click="addToPair(item)">{{item}}</button>
    </div> -->
    
    <p>Players</p>
    <ol>
      <li v-for="item in all">{{item}}</li>
    </ol>
    <br>
    <button class="random-btn" @click="randomize()">RANDOMIZE</button>
    <br>
    <br>
    <div>
      <div>
        <p>White Shirts</p>
        <hr>
        <p v-for="player in teamOneTemp">{{player}}</p>
      </div>

      <br>
      <div>
        <p>Black Shirts</p>
        <hr>
          <p v-for="player in teamTwoTemp">{{player}}</p>
      </div>
    </div>
  </div>
    <br>
    
  </div>

  
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { computed, watch } from "@vue/runtime-core";

let input = ref('');
let all = ref([]);
let restrictionPair = ref([]);
let restrictions = ref([]);
let teamOneTemp = ref([]);
let teamTwoTemp = ref([]);
let teamOne = ref([]);
let teamTwo = ref([]);

function addInput() {
  all.value.push(input.value.toUpperCase());
  input.value = '';
}

function addToPair(item) {
  restrictionPair.value.push(item);
}

const pairLength = computed(() => {
  return restrictionPair.value.length;
})


const teamSize = computed(() => {
  if(all.value.length % 2 === 0) {
    return all.value.length / 2;
  } else {
    return (all.value.length / 2) - 0.5;
  }
})

function shuffle(a) {
  let j, x, i;
  for (i = a.length - 1; i > 0; i--) {
        j = Math.floor(Math.random() * (i + 1));
        x = a[i];
        a[i] = a[j];
        a[j] = x;
    }
    return a;
}

function createTeams() {
  teamTwoTemp.value = shuffle([...all.value]);
  teamOneTemp.value = teamTwoTemp.value.splice(0, Math.floor(all.value.length / 2))
}

function randomize() {
  createTeams();

}

watch(pairLength, (value) => {
  console.log(value);
  if(value === 2) {
    restrictions.value.push(restrictionPair.value);
    restrictionPair.value = [];
  }
});
</script>

<style scoped>
.main-content {
  display: flex;
  justify-content: center;
}

.name-input {
  font-size:x-large;
  margin-bottom: 30px;
}
.random-btn {
  height: 10%;
}
</style>
