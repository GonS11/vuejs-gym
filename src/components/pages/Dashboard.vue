<script setup>
import Grid from '../Grid.vue';
import { gymHealthFacts } from '../../utils/index';

const props = defineProps({
  handleSelectWorkout: Function,
  firstIncompleteWorkoutIdx: Number,
  handleResetPlan: Function,
});

const randomNumber = Math.floor(Math.random() * gymHealthFacts.length);
</script>

<template>
  <section id="dashboard">
    <div class="card tip-container">
      <h2>Welcome user</h2>
      <p class="tip">
        <strong>Daily Tip</strong> <br />
        {{ gymHealthFacts[randomNumber] }}
      </p>
      <button
        @click="
          () =>
            handleSelectWorkout(
              firstIncompleteWorkoutIdx < 0 ? 0 : firstIncompleteWorkoutIdx,
            )
        "
      >
        Start workout &rarr;
      </button>
    </div>
    <Grid v-bind="props" />
  </section>
</template>

<style scoped>
.tip-container,
.tip-container div,
#dashboard {
  display: flex;
}

.tip-container,
#dashboard {
  flex-direction: column;
}

#dasboard {
  gap: 2rem;
}

.tip-container {
  gap: 0.5rem;
}

@media (min-width: 640px) {
  .tip-container {
    gap: 1rem;
  }
}
</style>
