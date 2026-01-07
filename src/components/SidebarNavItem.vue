<template>
  <v-list-item
    class="sidebar-nav-item"
    :class="{ 'active-item': active, collapsed: collapsed }"
    @click="$emit('click')"
    density="compact"
  >
    <v-icon class="sidebar-icon" :size="24">{{
      active && iconActive ? iconActive : icon
    }}</v-icon>
    <span v-if="!collapsed" class="sidebar-title">{{ title }}</span>
    <div v-if="active && !collapsed" class="active-bar"></div>
  </v-list-item>
</template>

<script setup>
defineProps({
  icon: String,
  iconActive: String,
  title: String,
  active: Boolean,
  collapsed: Boolean,
});
</script>

<style scoped>
.sidebar-nav-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding-left: 16px;
  border-radius: 8px;
  min-height: 48px;
  cursor: pointer;
  position: relative;
  transition: background 0.2s;
}
.sidebar-nav-item:hover {
  background-color: #f2f2f2;
}
.sidebar-nav-item.collapsed {
  justify-content: center;
  padding-left: 0;
}
.sidebar-icon {
  color: #212121;
}
.active-item {
  background-color: #f2f2f2 !important;
}
.active-bar {
  position: absolute;
  left: 0;
  top: 0;
  width: 4px;
  height: 100%;
  background-color: #ff0000;
  border-radius: 2px;
}
.sidebar-title {
  font-size: 16px;
  font-weight: 500;
  color: #212121;
}
</style>
