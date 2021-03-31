<script>
import firebase from 'firebase/app';
export default {
    mounted () {
        firebase.auth().signOut();
    }
}
</script>
