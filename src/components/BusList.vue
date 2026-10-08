<script setup>
import BusCard from './BusCard.vue'

defineProps({
  buses: {
    type: Array,
    required: true
  }
})

defineEmits(['edit', 'delete'])
</script>

<template>

  <section class="bus-section">

    <div class="section-heading">
      <div>
        <h3>Bus List</h3>
        <p>Registered Blue Way buses</p>
      </div>
    </div>

    <div v-if="buses.length > 0" class="bus-list">

      <BusCard
        v-for="bus in buses"
        :key="bus.id"
        :bus="bus"
        @edit="$emit('edit', $event)"
        @delete="$emit('delete', $event)"
      />

    </div>

    <div v-else class="empty">

      <div class="empty-icon">🚌</div>

      <h3>No buses found</h3>

      <p>
        Try another search or add a new bus.
      </p>

    </div>

  </section>

</template>