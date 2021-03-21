<template>
  <q-layout view="hHh lpR fFf">

    <q-header bordered class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="left = !left" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg">
          </q-avatar>
          TodoList - TeamSix
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="left" side="left" behavior="mobile" bordered>
      <q-scroll-area class="fit">
          <q-list>
            <q-item
              class="col-12"
              clickable
              v-ripple
              v-for="(menu) in menuList"
              :key="menu"
            >
              <q-session clickable @click="$router.replace(menu.link)">{{menu.label}}</q-session>
            </q-item>
          </q-list>
        </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view :key="$route.path"/>
    </q-page-container>

  </q-layout>
</template>

<script>
const menuList = [
  {
    icon: 'inbox',
    label: 'Tarefas',
    link: '/',
    separator: true
  },
  {
    icon: 'send',
    label: 'Condição',
    link: '/condicoes',
    separator: false
  }
]
export default {
  data () {
    return {
      left: false,
      menuList
    }
  }
}
</script>
