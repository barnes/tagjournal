<template>
  <q-page>
    <h1>Welcome to My Awesome App</h1>
    <q-card>
      <q-card-section>
        <div class="firebaseui-auth-container"></div>
        <div id="loader">Loading...</div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import firebase from 'firebase'


export default {
  name: 'PageIndex'
}
</script>

<style>
@import 'https://www.gstatic.com/firebasejs/ui/4.6.1/firebase-ui-auth.css'
</style>

