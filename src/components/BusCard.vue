<script setup>
defineProps({
  bus: {
    type: Object,
    required: true
  }
})

defineEmits(['edit', 'delete'])
</script>

<template>

  <article class="bus-card">

    <div class="bus-top">

      <div class="bus-symbol">
        🚌
      </div>

      <div class="bus-title">

        <h3>{{ bus.name }}</h3>

        <span class="bus-number">
          {{ bus.number }}
        </span>

      </div>

      <span
        class="status"
        :class="bus.status.toLowerCase()"
      >
        {{ bus.status }}
      </span>

    </div>

    <div class="bus-details">

      <div class="detail">

        <span class="detail-label">
          Route
        </span>

        <strong>
          {{ bus.route }}
        </strong>

      </div>

      <div class="detail">

        <span class="detail-label">
          Driver
        </span>

        <strong>
          {{ bus.driver }}
        </strong>

      </div>

      <div class="detail">

        <span class="detail-label">
          Schedule
        </span>

        <strong>
          {{ bus.schedule }}
        </strong>

      </div>

      <div class="detail">

        <span class="detail-label">
          Fare
        </span>

        <strong>
          ₱{{ Number(bus.fare).toFixed(2) }}
        </strong>

      </div>

    </div>

    <div class="bus-actions">

      <button
        class="view-button"
        @click="$emit('edit', bus)"
      >
        ✎ Edit
      </button>

      <button
        class="delete-button"
        @click="$emit('delete', bus.id)"
      >
        🗑 Delete
      </button>

    </div>

  </article>

</template>