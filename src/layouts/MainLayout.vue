<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="bg-primary text-white" height-hint="98">
      <q-toolbar>
          <h2>TagJournal</h2>
      </q-toolbar>

      <q-tabs align="right">
        <q-route-tab to="/" label="Home" />
        <q-route-tab to="/login" label="Login" />
        <q-route-tab to="/page2" label="My Journal" />
        <q-route-tab to="/page3" label="Logout" />
      </q-tabs>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>


export default {
  data () {
    return {
    }
  }
}
</script>