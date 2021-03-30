<template>
  <q-page>
    <Login></Login>
  </q-page>
</template>

<script>
import firebase from 'firebase'
import Login from '../components/Login'


export default {
  name: 'PageIndex',
  components: {
    Login,
  }
}
</script>

<style>
@import 'https://www.gstatic.com/firebasejs/ui/4.6.1/firebase-ui-auth.css'
</style>

