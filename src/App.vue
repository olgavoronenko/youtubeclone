<template>
  <v-app>
    <v-app-bar app flat class="bg-header">
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title class="text-h6 font-weight-bold">
        <span class="youtube-logo">YouTube</span>
      </v-toolbar-title>
      <v-spacer />
      <v-text-field
        hide-details
        prepend-inner-icon="mdi-magnify"
        label="Search"
        variant="outlined"
        density="compact"
        class="search-bar"
        @keyup.enter="handleClick('Search')"
      />
      <v-spacer />
      <v-btn icon @click="handleClick('Upload')">
        <v-icon>mdi-video-plus</v-icon>
      </v-btn>
      <v-btn icon @click="handleClick('Notifications')">
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn icon @click="handleClick('Profile')">
        <v-icon>mdi-account-circle</v-icon>
      </v-btn>
    </v-app-bar>
    <v-navigation-drawer
      v-model="drawer"
      app
      :permanent="true"
      :width="drawer ? 240 : 80"
      class="bg-sidebar"
      :clipped="$vuetify.display.smAndDown"
    >
      <v-list nav>
        <SidebarNavItem
          v-for="item in navItems"
          :key="item.nav"
          :icon="item.icon"
          :icon-active="item.iconActive"
          :title="item.title"
          :active="selectedNav === item.nav"
          :collapsed="!drawer"
          @click="selectNav(item.nav)"
        />
      </v-list>
      <template v-if="drawer">
        <v-divider class="my-2" />
        <div class="sidebar-extras">
          <div class="px-4 py-2 text-grey">
            Sign in to like videos,<br />
            comment and subscribe.
            <v-btn
              variant="outlined"
              class="mt-2"
              size="small"
              prepend-icon="mdi-account-circle"
              @click="handleClick('Sign in')"
            >
              Sign in
            </v-btn>
          </div>
          <v-divider class="my-2" />
          <div class="px-4 py-1 text-grey font-weight-bold section-title">
            Explore
          </div>
          <SidebarNavItem
            icon="mdi-music"
            title="Music"
            :collapsed="false"
            @click="selectNav('Music')"
          />
          <SidebarNavItem
            icon="mdi-gamepad-variant"
            title="Gaming"
            :collapsed="false"
            @click="selectNav('Gaming')"
          />
          <SidebarNavItem
            icon="mdi-trophy"
            title="Sport"
            :collapsed="false"
            @click="selectNav('Sport')"
          />
          <SidebarNavItem
            icon="mdi-school"
            title="Courses"
            :collapsed="false"
            @click="selectNav('Courses')"
          />
          <v-divider class="my-2" />
          <div class="px-4 py-1 text-grey font-weight-bold section-title">
            More from YouTube
          </div>
          <SidebarNavItem
            icon="mdi-youtube"
            title="YouTube Premium"
            :collapsed="false"
            @click="selectNav('YouTube Premium')"
          />
          <SidebarNavItem
            icon="mdi-music-circle"
            title="YouTube Music"
            :collapsed="false"
            @click="selectNav('YouTube Music')"
          />
          <SidebarNavItem
            icon="mdi-baby-face"
            title="YouTube Kids"
            :collapsed="false"
            @click="selectNav('YouTube Kids')"
          />
          <v-divider class="my-2" />
          <SidebarNavItem
            icon="mdi-cog"
            title="Settings"
            :collapsed="false"
            @click="selectNav('Settings')"
          />
          <SidebarNavItem
            icon="mdi-flag"
            title="Report history"
            :collapsed="false"
            @click="selectNav('Report history')"
          />
          <SidebarNavItem
            icon="mdi-help-circle-outline"
            title="Help"
            :collapsed="false"
            @click="selectNav('Help')"
          />
        </div>
      </template>
    </v-navigation-drawer>
    <v-main class="bg-main">
      <div class="feed-filter px-4 py-2">
        <div class="scroll-container">
          <div v-for="(tag, i) in tags" :key="i" class="tag-wrapper">
            <v-chip
              class="tag-chip"
              :class="{ 'active-chip': selectedTag === tag }"
              filter
              @click="selectTag(tag)"
              size="large"
            >
              {{ tag }}
            </v-chip>
          </div>
        </div>
      </div>
      <v-container fluid class="py-4">
        <v-row dense>
          <v-col
            v-for="video in allVideos"
            :key="video.id"
            cols="12"
            sm="6"
            md="3"
            lg="2"
          >
            <v-card
              flat
              class="video-card"
              @click="handleClick(video.title)"
              hover
            >
              <div class="thumbnail"></div>
              <v-card-text>
                <div class="text-subtitle-2 text-dark">{{ video.title }}</div>
                <div class="text-body-2 text-grey">
                  {{ video.channel }} • {{ video.views }} • {{ video.time }}
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from "vue";
import SidebarNavItem from "./components/SidebarNavItem.vue";

const navItems = [
  {
    nav: "Home",
    title: "Home",
    icon: "mdi-home-outline",
    iconActive: "mdi-home",
  },
  {
    nav: "Shorts",
    title: "Shorts",
    icon: "mdi-video-outline",
    iconActive: "mdi-video-outline",
  },
  {
    nav: "Subscriptions",
    title: "Subscriptions",
    icon: "mdi-play-box-multiple-outline",
    iconActive: "mdi-play-box-multiple-outline",
  },
  {
    nav: "You",
    title: "You",
    icon: "mdi-account-circle-outline",
    iconActive: "mdi-account-circle-outline",
  },
  {
    nav: "History",
    title: "History",
    icon: "mdi-history",
    iconActive: "mdi-history",
  },
];
const selectedNav = ref("Home");
const selectedTag = ref("All");
const drawer = ref(true);
const tags = [
  "All",
  "Music",
  "Cooking",
  "Gaming",
  "News",
  "Sports",
  "Comedy",
  "Movies",
  "Travel",
  "Technology",
  "DIY",
  "Education",
  "Live",
  "Podcasts",
  "Animals",
  "Streaming",
  "League of Legends",
  "Valorant",
  "Dota2",
  "not porn",
];
const allVideos = Array.from({ length: 30 }, (_, i) => ({
  id: i + 1,
  title: `Cool video`,
  channel: `Channel`,
  views: `1m views`,
  time: `2435 days ago`,
}));
function handleClick(item) {
  console.log(`Clicked: ${item}`);
}
function selectNav(nav) {
  selectedNav.value = nav;
}
function selectTag(tag) {
  selectedTag.value = tag;
}
</script>

<style scoped>
.bg-header {
  background-color: #fff !important;
  box-shadow: none;
  border-bottom: 1px solid #e5e5e5;
}
.bg-sidebar {
  background-color: #fff !important;
  border-right: 1px solid #e5e5e5;
}
.bg-main {
  background-color: #f8f8f8 !important;
}
.youtube-logo {
  font-size: 22px;
  color: #ff0000;
  font-weight: bold;
  letter-spacing: -1px;
}
.search-bar {
  max-width: 600px;
  flex: 1;
}
.feed-filter {
  border-bottom: 1px solid #e5e5e5;
  background: #fff;
  position: sticky;
  top: 64px;
  z-index: 1;
  display: flex;
  align-items: center;
}
.scroll-container {
  display: flex;
  align-items: center;
  overflow-x: auto;
  white-space: nowrap;
  padding: 8px 0;
  gap: 8px;
  scrollbar-width: thin;
}
.scroll-container::-webkit-scrollbar {
  height: 4px;
}
.scroll-container::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.12);
  border-radius: 3px;
}
.scroll-container::-webkit-scrollbar-track {
  background: transparent;
}
.tag-wrapper {
  display: flex;
}
.tag-chip {
  background: #f2f2f2;
  color: #212121;
  font-weight: 500;
  border-radius: 999px;
  height: 36px !important;
  padding: 0 12px !important;
  white-space: nowrap;
  display: flex;
  align-items: center;
  justify-content: center;
}
.active-chip {
  background: #212121 !important;
  color: #fff !important;
}
.video-card {
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.08);
  margin-bottom: 16px;
  transition: box-shadow 0.2s;
}
.video-card:hover {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.12);
}
.text-dark {
  color: #212121 !important;
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  line-height: 1.2em;
  height: 2.4em;
}
.text-grey {
  color: #606060 !important;
  font-size: 13px;
}
.thumbnail {
  width: 100%;
  aspect-ratio: 16/9;
  background-color: #ddd;
  border-radius: 12px;
}
.sidebar-extras {
  margin-top: 16px;
}
.section-title {
  font-size: 14px;
  color: #606060;
  margin-bottom: 8px;
}
</style>
