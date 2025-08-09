<script setup>
import Welcome from './components/pages/Welcome.vue';
import Workout from './components/pages/Workout.vue';
import Dashboard from './components/pages/Dashboard.vue';
import Layout from './components/layouts/Layout.vue';
import { computed, onMounted, ref } from 'vue';
import { workoutProgram } from './utils';

const defaultData = {};
for (let workoutIdx in workoutProgram) {
  const workoutData = workoutProgram[workoutIdx];
  defaultData[workoutIdx] = {};

  for (let e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = '';
  }
}

const selectedDisplay = ref(1);
const data = ref(defaultData);
const selectedWorkout = ref(-1);

const isWorkoutComplete = computed(() => {
  const currentWorkout = data.value?.[selectedWorkout.value];
  if (!currentWorkout) return false;

  const isCompleteCheck = Object.values(currentWorkout).every((ex) => !!ex);
  return isCompleteCheck;
});

const firstIncompleteWorkoutIdx = computed(() => {
  const allWorkouts = data.value;
  if (!allWorkouts) return -1;

  for (const [index, workout] of Object.entries(allWorkouts)) {
    const isComplete = Object.values(workout).every((ex) => !!ex);

    if (!isComplete) {
      return parseInt(index);
    }
  }

  return -1;
});

function handleChangeDisplay(idx) {
  selectedDisplay.value = idx;
}

function handleSelectWorkout(idx) {
  selectedDisplay.value = 3;
  selectedWorkout.value = idx;
}

function handleSaveWorkout() {
  localStorage.setItem('workouts', JSON.stringify(data.value));

  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
}

function handleResetPlan() {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage.removeItem('workouts');
}

onMounted(() => {
  if (!localStorage) return;

  if (localStorage.getItem('workouts')) {
    const savedData = JSON.parse(localStorage.getItem('workouts'));

    data.value = savedData;
    selectedDisplay.value = 2;
  }
});
</script>

<template>
  <Layout>
    <Welcome
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay === 1"
    />
    <Dashboard
      :handleSelectWorkout="handleSelectWorkout"
      :firstIncompleteWorkoutIdx="firstIncompleteWorkoutIdx"
      v-if="selectedDisplay === 2"
    />
    <Workout
      :data="data"
      :selectedWorkout="selectedWorkout"
      :isWorkoutComplete="isWorkoutComplete"
      :handleSaveWorkout="handleSaveWorkout"
      v-if="workoutProgram?.[selectedWorkout]"
    />
  </Layout>
</template>

<style scoped></style>
