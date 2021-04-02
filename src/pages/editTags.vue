<template>
    <q-page>
        <h1>Edit Tags</h1>
        <q-list>
            <q-list-item>
                Test Tag
                <q-btn label="delete"/>
            </q-list-item>
            <q-list-item v-for="tag in tags" :key="tag.id">
                {{ tag.tag }}
                <q-btn label="delete" @click="deleteTag(tag.id)"/>
            </q-list-item>
        </q-list>
    </q-page>
</template>

<script>
import firebase from 'firebase/app';
import db from '../boot/firebase';
export default {
  name: 'journalEntry',
  data () {
    return {
      loggedIn: false,
      inputEntry: '',
      uid: '',
      tags: [

      ],
      newTag: {
          tag: '',
      },
    }
  },
  methods: {

        addTagToDB() {
            this.newTag.tag = this.inputEntry;
            this.newTag.display = false; 
            db.collection("users").doc(this.uid).collection("tags").add(this.newTag)
            .then((docRef) => {
                console.log("Document written with ID: ", docRef.id)
                this.inputEntry= '';
            })
            .catch((error) => {
                console.error("Error adding document: ", error)
            });
        },
        deleteTag(inputID){
            let index = this.tags.findIndex(tag => tag.id === inputID );
            db.collection("users").doc(this.uid).collection("tags").doc(inputID).delete().then(() => {
                console.log("Tag deleted: "+ inputID);
            }).catch((error) => {
                console.error("Error removing document: ", error);
            });
        
        },
    },
    mounted() {

        firebase.auth().onAuthStateChanged((user) => {
        if (user) {
            console.log("GETTING TAGS:")
            console.log(this.tags);
            this.loggedIn = true;
            this.uid = user.uid;
            console.log(this.uid);
            db.collection("users").doc(this.uid).collection("tags")
                .onSnapshot((snapshot) => {
                snapshot.docChanges().forEach((change) => {
                let tagChange = change.doc.data()
                tagChange.id = change.doc.id
                    if (change.type === "added") {
                        console.log("New tag ", tagChange);
                        this.tags.unshift(tagChange);
                        console.log(this.tags);

                    }
                    if (change.type === "modified") {
                        console.log("Modified tag: ", tagChange);
                    }
                    if (change.type === "removed") {
                        console.log("Removed tag: ", tagChange);
                        let index = this.tags.findIndex(tag => tag.id === tagChange.id);
                        this.tags.splice(index,1)
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

