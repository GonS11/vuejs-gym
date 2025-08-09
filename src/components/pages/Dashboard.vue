<script setup>
import Grid from '../Grid.vue';
import { gymHealthFacts } from '../../utils/index';

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
      <button>Start workout &rarr;</button>
    </div>
    <Grid />
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
