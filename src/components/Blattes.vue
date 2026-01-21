<script setup>
import { onMounted, ref, watchEffect } from "vue";
import OBR from "@owlbear-rodeo/sdk";

const ID = "com.tutorial.blatteBag";

function updateData() {
  console.log("Sending data...");
  OBR.room.setMetadata({
    [ID + "/" + "bag"]: bag,
    [ID + "/" + "blattesArray"]: blattesArray,
  });
  console.log("Data sent ! " + OBR.room.getMetadata()[ID + "/" + "bag"]);
}

onMounted(() => {
  resetBag();
});

watchEffect(() => {
  OBR.room.onMetadataChange((metadata) => {
    console.log("Receiving data...");
    bag = metadata[ID + "/" + "bag"];
    blattesArray = metadata[ID + "/" + "blattesArray"];
    console.log("Data received !");
  });
});

const bagNames = [
  '<p style="color:black;">Blatte noire (échec critique)</p>',
  '<p style="color:red;">Blatte rouge (réussite critique)</p>',
  '<p style="color:blue;">Blatte bleue (réussite)</p>',
  '<p style="color:green;">Blatte verte (réussite améliorée)</p>',
  '<p style="color:grey;">Blatte blanche (échec)</p>',
];
const bagNumbers = [3, 3, 6, 6, 12];

let bag = [];
let blattesArray = [];
const blattesString = ref("");
const bagCount = ref(0);

function resetBag() {
  bag = [];
  blattesArray = [];
  blattesString.value = "";
  for (let i = 0; i < bagNumbers.length; i++) {
    for (let j = 0; j < bagNumbers[i]; j++) {
      bag.push(bagNames[i]);
    }
  }
  bagCount.value = bag.length;
  updateData();
}

function draw() {
  shuffle(bag);
  blattesArray.push(bag.pop());
  blattesString.value = blattesArray.toString();
  bagCount.value = bag.length;
  updateData();
}

function shuffle(array) {
  let currentIndex = array.length;

  // While there remain elements to shuffle...
  while (currentIndex != 0) {
    // Pick a remaining element...
    let randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    // And swap it with the current element.
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex],
      array[currentIndex],
    ];
  }
}
</script>

<template>
  <div
    class="result"
    v-html="blattesString.replace(new RegExp(',', 'g'), '')"
  ></div>
  <div class="draw">
    <button @click="draw">Tirer une blatte</button>
  </div>
  <div class="reset">
    <button @click="resetBag">Ranger les blattes</button>
  </div>
  <div class="content">Blattes dans le sac : {{ bagCount }}</div>
</template>

<style scoped>
.result {
  width: 250px;
  height: 250px;
  align-content: flex-end;
}

.draw,
.result,
.reset,
.content {
  text-align: center;
}

button {
  margin: 10px;
  height: 50px;
}
</style>
