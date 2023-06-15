<template>
  <q-layout view="lHh Lpr lFf">
    <!-- <q-header elevated :class="$q.dark.isActive ? 'bg-secondary' : 'bg-black'">
        <q-toolbar>
          <q-btn flat @click="drawer = !drawer" round dense icon="menu" />
          <q-toolbar-title>Header</q-toolbar-title>
        </q-toolbar>
      </q-header> -->
    <!-- <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header> -->
    <!-- <q-header elevated :class="$q.dark.isActive ? 'bg-secondary' : 'bg-black'">
        <q-toolbar>
          <q-btn flat @click="drawer = !drawer" round dense icon="menu" />
          <q-toolbar-title>Header</q-toolbar-title>
        </q-toolbar>
      </q-header> -->

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list class="q-ma-lg">
     <h2>YOR POS</h2>
<!-- <q-btn clickable @click="$router.push('/home')">Home </q-btn>
<q-btn clickable @click="$router.push('/sign-in')">Login</q-btn>
<q-btn clickable @click="$router.push('/register')">Register </q-btn>
<q-btn clickable @click="$router.push('/feed')">Feed </q-btn> -->

        <EssentialLink
        class="q-ma-xl"
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'

// const linksList = [
//   {
//     title: 'Home',
//     caption: 'Home',
//     icon: 'home',
//     link: 'https://erickedkiban.github.io/estore/#/home'
//   },
//   {
//     title: 'Menu',
//     caption: 'Menu',
//     icon: 'menu',
//     link: 'https://erickedkiban.github.io/estore/#/menu'
//   },

// ]
const linksList = [
  {
    title: 'HOME',
    icon: 'home',
    link: 'http://localhost:8080/#/home'
  },
  {
    title: 'MENU',
    icon: 'menu',
    link: 'http://localhost:8080/#/menu'
  },
  {
    title: 'PAYMENT',
    icon: 'payment',
    link: 'http://localhost:8080/#/home'
  },
  {
    title: 'ORDERS',
    icon: 'dashboard',
    link: 'http://localhost:8080/#/menu'
  },
  {
    title: 'SETTINGS',
    icon: 'settings',
    link: 'http://localhost:8080/#/home'
  },
  {
    caption: 'V 1.0',
  },

]

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
