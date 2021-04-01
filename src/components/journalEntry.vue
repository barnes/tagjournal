<!-- Simple form with buttons for tags and a way to create new tags. Simple text field and enter to create new tags.
Render tags as soon as added. -->

<template>
    <q-page>
        <q-input v-on:keyup.enter="addTag" square outlined v-model="inputEntry" label="Square outlined" />
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
      tags: [],
      newTag: {
          tag: ''
      }
    }
  },
  methods: {
        addTag() {
            this.newTag.tag = this.inputEntry;
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
            db.collection("tags").doc(inputID).delete().then(() => {
                console.log("Tag deleted: "+ inputID);
            }).catch((error) => {
                console.error("Error removing document: ", error);
            });
        
        },
        // following method is REQUIRED
        // (don't change its name --> "show")
        show () {
        this.$refs.dialog.show()
        },

        // following method is REQUIRED
        // (don't change its name --> "hide")
        hide () {
        this.$refs.dialog.hide()
        },

        onDialogHide () {
        // required to be emitted
        // when QDialog emits "hide" event
        this.$emit('hide')
        },

        onOKClick () {
        // on OK, it is REQUIRED to
        // emit "ok" event (with optional payload)
        // before hiding the QDialog
        this.$emit('ok')
        // or with payload: this.$emit('ok', { ... })

        // then hiding dialog
        this.hide()
        },

        onCancelClick () {
        // we just need to hide dialog
        this.hide()
        }
    },
    mounted() {

        firebase.auth().onAuthStateChanged((user) => {
        if (user) {
            this.loggedIn = true;
            this.uid = user.uid;
            console.log(this.uid);
        } else {
            this.loggedIn = false;
        }
        });

        db.collection("users").doc(this.uid).collection("tags")
        .onSnapshot((snapshot) => {
        snapshot.docChanges().forEach((change) => {
          let tagChange = change.doc.data()
          tagChange.id = change.doc.id
            if (change.type === "added") {
                console.log("New tag ", tagChange);
                this.tags.unshift(tagChange.tag);
                console.log(this.tags);

            }
            if (change.type === "modified") {
                console.log("Modified tag: ", tagChange);
            }
            if (change.type === "removed") {
                console.log("Removed tag: ", tagChange);
            }
        });
        });
  }
}


</script>

