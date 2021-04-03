<!-- Simple Journal Page. 
TODO: 
-Check if user is logged in. If logged in, get their collection/journal. 
-Create a simple dialog for launching a new entry component. 
-Render all previous journal entires with edit and delete options.-->

<template>
    <q-page>
        <h1>Journal Page</h1>
        <q-list>
            <q-list-item v-for="entry in journalEntires" :key="entry.id"> {{ entry }}</q-list-item>
        </q-list>
        <q-page-sticky position="bottom-right" :offset="[18, 18]">
            <q-btn fab icon="add" color="accent" label="Add Entry" @click="showEntryDialog"/>
        </q-page-sticky>
        <q-dialog ref="dialog" persistant>
            <journalEntry></journalEntry>
        </q-dialog>
    </q-page>
</template>

<script>
import journalEntry from '../components/journalEntry'
import firebase from 'firebase/app';
import db from '../boot/firebase';

export default {
    data () {
        return {
            uid: '',
            loggedIn: false,
            journalEntries: []
        }
    },
    components:{
        journalEntry
    },
    methods: {
        showEntryDialog () {
            this.$q.dialog({
            component: journalEntry
            })
        }   
    },
    mounted() {

        firebase.auth().onAuthStateChanged((user) => {
        if (user) {
            console.log("GETTING TAGS:")
            console.log(this.tags);
            this.loggedIn = true;
            this.uid = user.uid;
            console.log(this.uid);
            db.collection("users").doc(this.uid).collection("entries")
                .onSnapshot((snapshot) => {
                snapshot.docChanges().forEach((change) => {
                let entryChange = change.doc.data()
                entryChange.id = change.doc.id
                    if (change.type === "added") {
                        console.log("New entry ", entryChange);
                        this.journalEntries.unshift(entryChange);
                        console.log(this.journalEntries);

                    }
                    if (change.type === "modified") {
                        console.log("Modified entry: ", entryChange);
                    }
                    if (change.type === "removed") {
                        console.log("Removed entry: ", entryChange);
                        let index = this.journalEntires.findIndex(tag => tag.id === entryChange.id);
                        this.journalEntries.splice(index,1)
                    }
                });
                });
        } else {
            this.loggedIn = false;
        }
        });
    }
}
</script>

