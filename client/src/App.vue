<template>
  <div class="app">
    <!-- Sidebar -->
    <aside :class="['sidebar', { collapsed: sidebarCollapsed }]">
      <!-- Logo -->
      <div class="sidebar-logo">
        <!-- "CC" initials shown only when collapsed -->
        <div class="sidebar-logo-icon">CC</div>

        <!-- Full logo text shown when expanded -->
        <div class="sidebar-logo-text">
          <span class="sidebar-logo-name">{{ t('nav.companyName') }}</span>
          <span class="sidebar-logo-sub">{{ t('nav.subtitle') }}</span>
        </div>

        <!-- Toggle button: chevron-left when expanded, chevron-right when collapsed -->
        <button class="sidebar-toggle" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg v-if="!sidebarCollapsed" width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path d="M10 12L6 8L10 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <svg v-else width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path d="M6 4L10 8L6 12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <!-- Navigation -->
      <nav class="sidebar-nav">
        <span class="sidebar-nav-label">Navigation</span>

        <router-link to="/" class="sidebar-nav-item" :class="{ active: $route.path === '/' }" data-tooltip="Overview">
          <!-- Dashboard / Overview icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <rect x="2" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>

        <router-link to="/inventory" class="sidebar-nav-item" :class="{ active: $route.path === '/inventory' }" data-tooltip="Inventory">
          <!-- Inventory / Box icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <path d="M15 6H3V15C3 15.5523 3.44772 16 4 16H14C14.5523 16 15 15.5523 15 15V6Z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M2 3H16C16.5523 3 17 3.44772 17 4V6H1V4C1 3.44772 1.44772 3 2 3Z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 10H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link to="/orders" class="sidebar-nav-item" :class="{ active: $route.path === '/orders' }" data-tooltip="Orders">
          <!-- Orders / List icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <path d="M3 4H15M3 9H15M3 14H10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>

        <router-link to="/spending" class="sidebar-nav-item" :class="{ active: $route.path === '/spending' }" data-tooltip="Finance">
          <!-- Finance / Chart icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <path d="M3 14L7 9L10 12L14 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <circle cx="14" cy="6" r="1.5" fill="currentColor"/>
          </svg>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>

        <router-link to="/demand" class="sidebar-nav-item" :class="{ active: $route.path === '/demand' }" data-tooltip="Demand Forecast">
          <!-- Demand / Trend icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <path d="M2 13L6 8L9 11L13 5L16 7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M13 5H16V8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link to="/reports" class="sidebar-nav-item" :class="{ active: $route.path === '/reports' }" data-tooltip="Reports">
          <!-- Reports / Document icon -->
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" class="nav-icon">
            <path d="M10 2H4C3.44772 2 3 2.44772 3 3V15C3 15.5523 3.44772 16 4 16H14C14.5523 16 15 15.5523 15 15V7L10 2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M10 2V7H15" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M6 11H12M6 13H10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>

      <!-- Spacer pushes user section to bottom -->
      <div class="sidebar-spacer"></div>

      <!-- Bottom divider -->
      <div class="sidebar-divider"></div>

      <!-- User info -->
      <div class="sidebar-user">
        <div class="sidebar-user-avatar">
          {{ getInitials(currentUser.name) }}
        </div>
        <div class="sidebar-user-details">
          <div class="sidebar-user-name">{{ currentUser.name }}</div>
          <div class="sidebar-user-email">{{ currentUser.email }}</div>
        </div>
      </div>

      <!-- Language Switcher -->
      <div class="sidebar-lang">
        <LanguageSwitcher />
      </div>
    </aside>

    <!-- Main area -->
    <div :class="['main-area', { 'sidebar-collapsed': sidebarCollapsed }]">
      <!-- Top bar -->
      <header class="top-bar">
        <div class="top-bar-title">{{ currentPageTitle }}</div>
        <div class="top-bar-right">
          <FilterBar />
          <ProfileMenu
            @show-profile-details="showProfileDetails = true"
            @show-tasks="showTasks = true"
          />
        </div>
      </header>

      <!-- Page content -->
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <!-- Modals -->
    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser, getInitials } = useAuth()
    const { t } = useI18n()
    const route = useRoute()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    // Sidebar collapsed state — initialized in onMounted based on viewport width
    const sidebarCollapsed = ref(false)

    const toggleSidebar = () => {
      sidebarCollapsed.value = !sidebarCollapsed.value
    }

    // Only auto-collapse on resize; never auto-expand so manual expansion is respected
    const handleResize = () => {
      if (window.innerWidth < 1024) {
        sidebarCollapsed.value = true
      }
    }

    // Map routes to page titles for the top bar
    const pageTitles = {
      '/': 'Overview',
      '/inventory': 'Inventory',
      '/orders': 'Orders',
      '/spending': 'Finance',
      '/demand': 'Demand Forecast',
      '/reports': 'Reports'
    }

    const currentPageTitle = computed(() => {
      return pageTitles[route.path] || 'Dashboard'
    })

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      loadTasks()
      // Set initial collapsed state based on viewport width
      sidebarCollapsed.value = window.innerWidth < 1024
      window.addEventListener('resize', handleResize)
    })

    return {
      t,
      currentUser,
      getInitials,
      currentPageTitle,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      toggleSidebar
    }
  }
}
</script>

<style>
/* ===== Reset ===== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ===== App Shell ===== */
.app {
  display: flex;
  min-height: 100vh;
}

/* ===== Sidebar ===== */
.sidebar {
  width: 240px;
  min-width: 240px;
  background: #0f172a;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  z-index: 100;
  overflow-y: auto;
  overflow-x: hidden;
  /* Smooth width transition */
  transition: width 0.2s ease, min-width 0.2s ease;
}

/* Collapsed sidebar width */
.sidebar.collapsed {
  width: 60px;
  min-width: 60px;
}

.sidebar-logo {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 1.25rem 1.25rem 1rem;
  min-height: 64px;
  border-bottom: 1px solid #1e293b;
  gap: 0.5rem;
  /* Prevent content from wrapping during transition */
  overflow: hidden;
}

/* Center logo area contents when collapsed */
.sidebar.collapsed .sidebar-logo {
  justify-content: center;
  padding: 1.25rem 0 1rem;
}

/* "CC" initials — hidden by default, shown when collapsed */
.sidebar-logo-icon {
  display: none;
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
  color: #f8fafc;
  font-size: 0.6875rem;
  font-weight: 700;
  letter-spacing: 0.02em;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.sidebar.collapsed .sidebar-logo-icon {
  display: flex;
}

/* Full logo text — hidden when collapsed */
.sidebar-logo-text {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-width: 0;
  overflow: hidden;
  transition: opacity 0.15s ease, width 0.2s ease;
}

.sidebar.collapsed .sidebar-logo-text {
  opacity: 0;
  width: 0;
  overflow: hidden;
  pointer-events: none;
}

.sidebar-logo-name {
  font-size: 1rem;
  font-weight: 700;
  color: #f8fafc;
  letter-spacing: -0.02em;
  line-height: 1.3;
  white-space: nowrap;
}

.sidebar-logo-sub {
  font-size: 0.75rem;
  color: #94a3b8;
  font-weight: 400;
  margin-top: 1px;
  white-space: nowrap;
}

/* Toggle button placed at the right of the logo area */
.sidebar-toggle {
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  border-radius: 6px;
  color: #64748b;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  transition: background 0.15s ease, color 0.15s ease;
  flex-shrink: 0;
}

.sidebar-toggle:hover {
  background: #1e293b;
  color: #94a3b8;
}

/* When collapsed, toggle sits centered below the initials icon */
.sidebar.collapsed .sidebar-toggle {
  /* Keep toggle visible and accessible even when collapsed */
  margin: 0;
}

/* Nav section */
.sidebar-nav {
  padding: 1rem 0.75rem 0;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.sidebar-nav-label {
  display: block;
  font-size: 0.625rem;
  font-weight: 600;
  color: #475569;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  padding: 0 0.5rem;
  margin-bottom: 0.5rem;
  /* Smooth hide transition */
  transition: opacity 0.15s ease, height 0.2s ease, margin 0.2s ease;
  overflow: hidden;
  white-space: nowrap;
}

/* Hide nav label when collapsed */
.sidebar.collapsed .sidebar-nav-label {
  opacity: 0;
  height: 0;
  overflow: hidden;
  margin: 0;
}

.sidebar-nav-item {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  padding: 0 0.75rem;
  height: 36px;
  border-radius: 8px;
  font-size: 0.875rem;
  font-weight: 500;
  color: #94a3b8;
  text-decoration: none;
  transition: background 0.15s ease, color 0.15s ease, padding 0.2s ease, width 0.2s ease, margin 0.2s ease;
  border-left: 3px solid transparent;
  /* Offset the border so it doesn't shift content */
  margin-left: -3px;
  padding-left: calc(0.75rem + 3px);
  /* Required for tooltip positioning */
  position: relative;
}

.sidebar-nav-item:hover {
  color: #f8fafc;
  background: #1e293b;
}

.sidebar-nav-item.active {
  color: #eef2ff;
  background: #4338ca;
  border-left-color: #6366f1;
}

.sidebar-nav-item.active .nav-icon {
  color: #a5b4fc;
}

/* Center nav items when collapsed */
.sidebar.collapsed .sidebar-nav-item {
  justify-content: center;
  padding: 0;
  width: 40px;
  margin: 0 auto;
  border-left-color: transparent;
}

.sidebar.collapsed .sidebar-nav-item.active {
  border-left-color: transparent;
  border-radius: 8px;
}

.nav-icon {
  flex-shrink: 0;
  color: currentColor;
  opacity: 0.85;
}

/* Nav label text — hidden when collapsed */
.nav-label {
  transition: opacity 0.15s ease;
  white-space: nowrap;
  overflow: hidden;
}

.sidebar.collapsed .nav-label {
  opacity: 0;
  width: 0;
  overflow: hidden;
}

/* Bottom spacer */
.sidebar-spacer {
  flex: 1;
}

.sidebar-divider {
  height: 1px;
  background: #1e293b;
  margin: 0 0.75rem;
}

/* User info at bottom */
.sidebar-user {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.875rem 1rem;
  overflow: hidden;
  transition: padding 0.2s ease;
}

/* Center avatar when collapsed */
.sidebar.collapsed .sidebar-user {
  justify-content: center;
  padding: 0.875rem 0;
}

.sidebar-user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.75rem;
  letter-spacing: 0.025em;
  flex-shrink: 0;
}

/* User name + email wrapper — hidden when collapsed */
.sidebar-user-details {
  flex: 1;
  min-width: 0;
  overflow: hidden;
  transition: opacity 0.15s ease, width 0.2s ease;
}

.sidebar.collapsed .sidebar-user-details {
  opacity: 0;
  width: 0;
  overflow: hidden;
}

.sidebar-user-name {
  font-size: 0.8125rem;
  font-weight: 600;
  color: #f8fafc;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-user-email {
  font-size: 0.6875rem;
  color: #64748b;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-lang {
  padding: 0.5rem 1rem 1rem;
  overflow: hidden;
  transition: opacity 0.15s ease, height 0.2s ease, padding 0.2s ease;
}

/* Hide language switcher when collapsed */
.sidebar.collapsed .sidebar-lang {
  opacity: 0;
  height: 0;
  overflow: hidden;
  padding: 0;
}

/* ===== Tooltip for collapsed nav items ===== */
/* Pure CSS tooltip using data-tooltip attribute + ::after pseudo-element */
.sidebar.collapsed .sidebar-nav-item::after {
  content: attr(data-tooltip);
  position: absolute;
  left: calc(100% + 12px);
  top: 50%;
  transform: translateY(-50%);
  background: #0f172a;
  color: #f8fafc;
  font-size: 0.75rem;
  font-weight: 500;
  padding: 0.375rem 0.625rem;
  border-radius: 6px;
  white-space: nowrap;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.1s ease;
  z-index: 200;
  border: 1px solid #1e293b;
}

.sidebar.collapsed .sidebar-nav-item:hover::after {
  opacity: 1;
}

/* ===== Main Area ===== */
.main-area {
  margin-left: 240px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background: #f8fafc;
  /* Smooth offset transition matching sidebar width transition */
  transition: margin-left 0.2s ease;
}

/* Offset when sidebar is collapsed */
.main-area.sidebar-collapsed {
  margin-left: 60px;
}

/* ===== Top Bar ===== */
.top-bar {
  height: 60px;
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2rem;
  position: sticky;
  top: 0;
  z-index: 90;
  gap: 1rem;
}

.top-bar-title {
  font-size: 0.9375rem;
  font-weight: 600;
  color: #0f172a;
  white-space: nowrap;
  letter-spacing: -0.01em;
}

.top-bar-right {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
  justify-content: flex-end;
}

/* ===== Page Content ===== */
.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
}

/* ===== Page Header ===== */
.page-header {
  margin-bottom: 1.5rem;
}

.page-header h1,
.page-header h2 {
  font-size: 1.25rem;
  font-weight: 600;
  color: #0f172a;
  letter-spacing: -0.02em;
  margin-bottom: 0.25rem;
}

.page-header p {
  font-size: 0.875rem;
  color: #64748b;
  margin-top: 2px;
}

/* ===== Stats Grid ===== */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: #ffffff;
  padding: 1.25rem;
  border-radius: 12px;
  border: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
  transition: box-shadow 0.2s ease, border-color 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.stat-label {
  color: #64748b;
  font-size: 0.8125rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.stat-value {
  font-size: 1.75rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.03em;
  line-height: 1.1;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #4f46e5;
}

/* ===== Cards ===== */
.card {
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  border: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 0.9375rem;
  font-weight: 600;
  color: #0f172a;
  letter-spacing: -0.015em;
}

/* ===== Tables ===== */
.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.625rem 0.875rem;
  font-weight: 600;
  color: #64748b;
  font-size: 0.6875rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.625rem 0.875rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

/* ===== Badges ===== */
.badge {
  display: inline-block;
  padding: 0.25rem 0.625rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.01em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: #dbeafe;
  color: #1e40af;
}

/* ===== Loading / Error ===== */
.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.9375rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.9375rem;
}
</style>
