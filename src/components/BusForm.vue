<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  bus: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['save', 'cancel'])

const form = ref({
  name: '',
  number: '',
  route: '',
  driver: '',
  schedule: '',
  fare: '',
  status: 'Active'
})

const error = ref('')

watch(
  () => props.bus,
  (bus) => {

    if (bus) {
      form.value = {
        name: bus.name,
        number: bus.number,
        route: bus.route,
        driver: bus.driver,
        schedule: bus.schedule,
        fare: bus.fare,
        status: bus.status
      }
    } else {
      form.value = {
        name: '',
        number: '',
        route: '',
        driver: '',
        schedule: '',
        fare: '',
        status: 'Active'
      }
    }

    error.value = ''

  },
  { immediate: true }
)

function submitForm() {

  error.value = ''

  if (
    !form.value.name ||
    !form.value.number ||
    !form.value.route ||
    !form.value.driver ||
    !form.value.schedule ||
    form.value.fare === ''
  ) {
    error.value = 'Please complete all required fields.'
    return
  }

  if (Number(form.value.fare) < 0) {
    error.value = 'Fare cannot be negative.'
    return
  }

  const busData = {
    ...form.value,
    fare: Number(form.value.fare)
  }

  if (props.bus) {
    busData.id = props.bus.id
  }

  emit('save', busData)
}
</script>

<template>

  <div class="modal-background">

    <div class="modal">

      <div class="modal-header">

        <div>
          <p class="eyebrow">
            {{ bus ? 'UPDATE BUS' : 'CREATE BUS' }}
          </p>

          <h2>
            {{ bus ? 'Edit Bus Information' : 'Add New Bus' }}
          </h2>
        </div>

        <button
          class="close-button"
          @click="emit('cancel')"
        >
          ×
        </button>

      </div>

      <div
        v-if="error"
        class="form-error"
      >
        {{ error }}
      </div>

      <form @submit.prevent="submitForm">

        <div class="form-grid">

          <div class="form-group full">

            <label>Bus Name *</label>

            <input
              v-model="form.name"
              type="text"
              placeholder="Example: Blue Way Bohol Express"
            >

          </div>

          <div class="form-group">

            <label>Bus Number *</label>

            <input
              v-model="form.number"
              type="text"
              placeholder="Example: BW-001"
            >

          </div>

          <div class="form-group">

            <label>Route *</label>

            <input
              v-model="form.route"
              type="text"
              placeholder="Example: Tagbilaran - Clarin"
            >

          </div>

          <div class="form-group">

            <label>Driver *</label>

            <input
              v-model="form.driver"
              type="text"
              placeholder="Example: Juan Dela Cruz"
            >

          </div>

          <div class="form-group">

            <label>Schedule *</label>

            <input
              v-model="form.schedule"
              type="text"
              placeholder="Example: 7:00 AM"
            >

          </div>

          <div class="form-group">

            <label>Fare *</label>

            <input
              v-model="form.fare"
              type="number"
              min="0"
              step="0.01"
              placeholder="Example: 50"
            >

          </div>

          <div class="form-group">

            <label>Status *</label>

            <select v-model="form.status">

              <option value="Active">
                Active
              </option>

              <option value="Inactive">
                Inactive
              </option>

              <option value="Maintenance">
                Maintenance
              </option>

            </select>

          </div>

        </div>

        <div class="modal-actions">

          <button
            type="button"
            class="cancel-button"
            @click="emit('cancel')"
          >
            Cancel
          </button>

          <button
            type="submit"
            class="save-button"
          >
            {{ bus ? 'Update Bus' : 'Save Bus' }}
          </button>

        </div>

      </form>

    </div>

  </div>

</template>