<script setup>
import { ref, onMounted } from 'vue';
// @ts-ignore
import { getClassifiedAs, getPrimaryName, normalizeFieldToArray, getFieldValuesByClassifications } from "@thegetty/linkedart.js";

const name = ref("");
const bio = ref("");

const selectedWorks = ['4f1e53f9-71c5-42aa-889c-f45877908e14', 'bfaf34c9-713e-4f58-8e3e-277d5bd9b764', 'c8aa5b81-3263-4f30-aabd-7d4e791d05f4','24677302-55c2-445d-9a71-c2f3ceb051a7', '07b7f032-3e6b-40d7-b904-1524d2ddb81d', '28f3b598-2502-49d2-9cb0-48bbbedc48b3'];

const thumbnailUrl = ref([]);
const workTitles = ref([]);

const workData = ref([]);

const getLinkedArt = async (urls) => {
  const data = await Promise.all(
    urls.map(async (url) => {
      return await (await fetch("http://data.getty.edu/museum/collection/object/" + url)).json()})
  )
  return data;
}

function mapToWorkData(work) {
  return {
    id: work.id,
    title: getPrimaryName(work),
    thumbnailUrl: normalizeFieldToArray(work, "representation").map(r => r.id)[0],
    date: getFieldValuesByClassifications(work, "date")[0],
  };
}

onMounted (async () => {
  //get ansel adams person data
  const response = await fetch("https://data.getty.edu/museum/collection/person/bbe60198-3dc4-4c80-b775-a7c17f35cace");
  const anselAdams = await response.json();
  console.log(anselAdams);
  
  name.value = getPrimaryName(anselAdams);
  bio.value = anselAdams.referred_to_by[0].content;
  console.log(bio.value);

  //get selected works
  const linkedArtData = await getLinkedArt(selectedWorks);
  //map to workData array
  workData.value = linkedArtData.map(mapToWorkData);

  console.log("classifications: ", workData.value.map(work => work.classification));

  console.log("dates: ", workData.value.map(work => work.date));

});


</script>

<template>
  <div class="main-content pl-60 pr-[100px] py-[100px]">
    <div class="max-w-[78rem] mx-auto">
      <h1 class="text-[42px] font-display">Selected Works by {{ name }}</h1>
      <p class="pt-[12px] content">{{ bio }}</p>
      <div class="work-grid pt-[70px] flex flex-wrap items-end justify-between gap-[40px]">
        <div v-for="work in workData" :key="work.id">
          <img :src="work.thumbnailUrl" :alt="work.title" />
          <h2 class="text-[22px] pt-[16px]">{{ work.title }}</h2>
          <date>{{ work.date }}</date>
        </div>
      </div>

      <div class="links">
        <h2 class="font-display text-[32px] pt-[70px] pb-[12px]">Resources Used</h2>
        <a class="block underline pb-[3px]" href="https://observablehq.com/@jrladd/linked-art-1" target="_blank" rel="noopener noreferrer">Understanding Linked Art</a>
        <a class="block underline pb-[3px]" href="https://linkedartjs.org/module-LinkedArtHelpers.html#.getFieldValuesByClassifications" target="_blank" rel="noopener noreferrer">LinkedArt.js Helpers</a>
        <a class="block underline" href="https://www.getty.edu/art/collection/person/103KE7" target="_blank" rel="noopener noreferrer">Ansel Adams at the Getty</a>
      </div>
      <p class="text-[14px] pt-[20px]">The text on this page is licensed under a <a class="underline" href="https://creativecommons.org/licenses/by/4.0/" target="_blank" rel="noopener noreferrer">Creative Commons Attribution 4.0 International License</a>, unless otherwise noted. Images and other media are excluded.</p>
    </div>
  </div>
</template>
