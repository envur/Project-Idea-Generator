<template>
  <div>
    <section class="text-center text-white font-bold py-6 bg-gradient-to-br from-green-600 to-teal-300">
      <div class="container">
        <h1 class="text-2xl pb-5">
          What skills do you want to work on?
        </h1>
        <div v-for="skill in skillList" :key="skill.id">
          <label :for="skill.id">
            <input type="checkbox" v-model="selectedSkills" @change="generateFilteredAppList" :value="skill.id" :id="skill.id">
            {{ skill.skill }}
          </label>
        </div>
      </div>
    </section>

    <div class="container">
      <div class="grid md:pl-12 grid-cols-1 md:grid-cols-3 gap-3">
        <div v-for="app in filteredAppList" :key="app.id">
          <div class="min-h-full flex justify-center">
            <div class="block p-6 rounded-lg shadow-lg bg-white max-w-sm">
              <div class="py-3 text-xl uppercase px-6 border-b border-gray-300">
                {{ app.app }}
              </div>
              <div class="p-6">
                <p>{{ app.instructions }}</p>
                <h4 class="text-lg my-2">
                  <b>Skills</b>:
                </h4>
                <ul v-for="skill in app.skills" :key="skill.id">
                  <li>
                    <strong>{{ skillList[skill-1].skill }}</strong>
                    <p v-if="skillList[skill-1].options" :set="randSkill = getRand(skillList[skill-1].options)">
                      🦮 <a :href="randSkill" target="_blank">{{ randSkill }}</a>
                    </p>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IdeasGenerator',
}
</script>

<script setup>
import '../assets/tailwind.css';
import { ref } from 'vue';

const GENERATOR_BASE = 'http://localhost:3000';
const skillList = ref([]);
const selectedSkills = ref([]);
const filteredAppList = ref([]);
let appList = [];

async function getSkillList() {
  const response = await fetch(`${GENERATOR_BASE}/skills`);
  skillList.value = await response.json();
}

async function getAppList() {
  const response = await fetch(`${GENERATOR_BASE}/apps`);
  appList = await response.json();
  filteredAppList.value = appList;
}

function generateFilteredAppList() {
  filteredAppList.value = [];

  for (const app of appList) {
    const appSkillsArray = app.skills;
    const selectedSkillsArray = selectedSkills.value;

    if (hasAllSkills(appSkillsArray, selectedSkillsArray)) {
      filteredAppList.value.push(app);
    }
  }
}

function hasAllSkills(appSkills, selectedSkills) {
  return selectedSkills.every(f => appSkills.includes(f));
}

function getRand(value) {
  let keys = Object.keys(value);
  return value[keys[ keys.length * Math.random() << 0 ]];
}

getSkillList();
getAppList();

</script>
