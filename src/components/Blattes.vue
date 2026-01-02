<script setup>
import { ref } from "vue";
import BlattesImage from "@/components/BlattesImage.vue";

defineProps({
  msg: {
    type: String,
    required: true,
  },
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

function resetBag() {
  bag = [];
  blattesArray = [];
  blattesString.value = "";
  for (let i = 0; i < bagNumbers.length; i++) {
    for (let j = 0; j < bagNumbers[i]; j++) {
      bag.push(bagNames[i]);
    }
  }
}

function draw() {
  shuffle(bag);
  console.log(bag);
  blattesArray.push(bag.pop());
  blattesString.value = blattesArray.toString();
  console.log(blattesArray);
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

function getImage(image) {
  return require(image);
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
</template>

<style scoped>
.result {
  width: 250px;
  height: 250px;
  align-content: flex-end;
}

.draw,
.result,
.reset {
  text-align: center;
}
</style>
