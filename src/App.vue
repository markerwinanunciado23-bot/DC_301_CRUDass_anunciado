<script setup>
import { ref, computed } from 'vue'
import BusForm from './components/BusForm.vue'
import BusList from './components/BusList.vue'

const buses = ref([
  {
    id: 1,
    name: 'Blue Way Bohol Express',
    number: 'BW-001',
    route: 'Tagbilaran - Clarin',
    driver: 'Juan Dela Cruz',
    schedule: '7:00 AM',
    fare: 50,
    status: 'Active'
  },
  {
    id: 2,
    name: 'Blue Way Provincial',
    number: 'BW-002',
    route: 'Tagbilaran - Tubigon',
    driver: 'Pedro Santos',
    schedule: '8:30 AM',
    fare: 60,
    status: 'Active'
  },
  {
    id: 3,
    name: 'Blue Way Island Transport',
    number: 'BW-003',
    route: 'Tagbilaran - Jagna',
    driver: 'Carlos Reyes',
    schedule: '9:00 AM',
    fare: 80,
    status: 'Active'
  },
  {
    id: 4,
    name: 'Blue Way Transit',
    number: 'BW-004',
    route: 'Tagbilaran - Ubay',
    driver: 'Michael Garcia',
    schedule: '10:00 AM',
    fare: 100,
    status: 'Maintenance'
  },
  {
    id: 5,
    name: 'Blue Way Express',
    number: 'BW-005',
    route: 'Tagbilaran - Carmen',
    driver: 'Mark Villanueva',
    schedule: '11:30 AM',
    fare: 70,
    status: 'Active'
  },
  {
    id: 6,
    name: 'Blue Way Shuttle',
    number: 'BW-006',
    route: 'Tagbilaran - Panglao',
    driver: 'Jose Mendoza',
    schedule: '1:00 PM',
    fare: 40,
    status: 'Active'
  },
  {
    id: 7,
    name: 'Blue Way Bus Service',
    number: 'BW-007',
    route: 'Tagbilaran - Anda',
    driver: 'Antonio Ramos',
    schedule: '2:30 PM',
    fare: 120,
    status: 'Inactive'
  }
])

const showForm = ref(false)
const editingBus = ref(null)
const search = ref('')

const filteredBuses = computed(() => {
  const keyword = search.value.toLowerCase().trim()

  if (!keyword) {
    return buses.value
  }

  return buses.value.filter(bus =>
    bus.name.toLowerCase().includes(keyword) ||
    bus.number.toLowerCase().includes(keyword) ||
    bus.route.toLowerCase().includes(keyword) ||
    bus.driver.toLowerCase().includes(keyword)
  )
})

function openAddForm() {
  editingBus.value = null
  showForm.value = true
}

function addBus(bus) {
  const newBus = {
    ...bus,
    id: Date.now()
  }

  buses.value.push(newBus)
  showForm.value = false
}

function openEditForm(bus) {
  editingBus.value = { ...bus }
  showForm.value = true
}

function updateBus(updatedBus) {
  const index = buses.value.findIndex(bus => bus.id === updatedBus.id)

  if (index !== -1) {
    buses.value[index] = updatedBus
  }

  showForm.value = false
  editingBus.value = null
}

function deleteBus(id) {
  const bus = buses.value.find(bus => bus.id === id)

  if (!bus) return

  const confirmed = confirm(
    `Are you sure you want to delete "${bus.name}"?`
  )

  if (confirmed) {
    buses.value = buses.value.filter(bus => bus.id !== id)
  }
}

function closeForm() {
  showForm.value = false
  editingBus.value = null
}
</script>

<template>
  <div class="app">

    <header class="header">
      <div class="brand">
        <div class="logo">🚌</div>

        <div>
          <h1>Blue Way</h1>
          <p>Bus Management System</p>
        </div>
      </div>

      <div class="header-info">
        <span>CRUD Application</span>
      </div>
    </header>

    <main class="container">

      <section class="hero">
        <div>
          <p class="eyebrow">BUS INFORMATION SYSTEM</p>

          <h2>Manage Blue Way Buses</h2>

          <p class="hero-text">
            Add, view, update, search, and delete bus information
            using this simple CRUD application.
          </p>
        </div>

        <button class="add-button" @click="openAddForm">
          + Add New Bus
        </button>
      </section>

      <section class="stats">

        <div class="stat-card">
          <div class="stat-icon">🚌</div>

          <div>
            <span>Total Buses</span>
            <strong>{{ buses.length }}</strong>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">✓</div>

          <div>
            <span>Active</span>
            <strong>
              {{ buses.filter(bus => bus.status === 'Active').length }}
            </strong>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">⚙</div>

          <div>
            <span>Maintenance</span>
            <strong>
              {{ buses.filter(bus => bus.status === 'Maintenance').length }}
            </strong>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">✕</div>

          <div>
            <span>Inactive</span>
            <strong>
              {{ buses.filter(bus => bus.status === 'Inactive').length }}
            </strong>
          </div>
        </div>

      </section>

      <section class="toolbar">

        <div class="search-box">
          <span>🔍</span>

          <input
            v-model="search"
            type="text"
            placeholder="Search bus, number, route, or driver..."
          >
        </div>

        <div class="result-count">
          {{ filteredBuses.length }} bus{{ filteredBuses.length === 1 ? '' : 'es' }}
        </div>

      </section>

      <BusList
        :buses="filteredBuses"
        @edit="openEditForm"
        @delete="deleteBus"
      />

    </main>

    <BusForm
      v-if="showForm"
      :bus="editingBus"
      @save="editingBus ? updateBus($event) : addBus($event)"
      @cancel="closeForm"
    />

    <footer>
      <p>Blue Way Bus Information System • CRUD Functionality Assignment</p>
    </footer>

  </div>
</template>