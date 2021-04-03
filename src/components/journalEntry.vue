<!-- Simple form with buttons for tags and a way to create new tags. Simple text field and enter to create new tags.
Render tags as soon as added. -->

<template>
    <q-dialog ref="dialog" @hide="onDialogHide">
        <q-card>
            <q-card-section>
                <p>Select the tags that apply to your day. If you want to add a new tag, type it here and press enter</p>
                <q-input v-on:keyup.enter="addTagToDB" square outlined v-model="inputEntry" label="Enter new tag, press enter." />
                <q-btn v-for="tag in tags" :label="tag.tag" :key="tag.id"  size="lg" @click="addTag(tag)"/>
                <q-separator />
                <h2>Your Entry</h2>
                <q-btn v-for="entry in newEntry" :key="entry.id" disable :label="entry.tag" />
                <q-separator />
                <q-btn @click="submitEntry" label="submit" size="xl"/>
            </q-card-section>
        </q-card>
    </q-dialog>
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
      newEntry: [

      ],
      newEntryObj: {}

    }
  },
  methods: {
        addTag(tag){
            //display false -> not currently in entry
            if(tag.display === false){
                this.newEntry.unshift(tag);
                //TODO: Set display to true in the parent tag object.
                let index = this.tags.findIndex(tagIn => tagIn.id === tag.id)
                this.tags[index].display = true;
            }
            //display true -> currently in entry
            else {
                //TODO: Remove the clicked button from new entry.
                let index = this.newEntry.findIndex(tagIn => tagIn.id === tag.id);
                this.newEntry.splice(index,1);
                let index2 = this.tags.findIndex(tagIn => tagIn.id === tag.id)
                this.tags[index2].display = false;
            }
            
        },

        submitEntry() {
            this.newEntry.created = Date.now();
            this.newEntryObj = Object.assign({},this.newEntry);
            console.log(this.newEntryObj);
            db.collection("users").doc(this.uid).collection("entries").add(this.newEntryObj)
            .then((docRef) => {
                console.log("Document entry written");
                this.newEntry = [];
                this.newEntryObj = {};
            })
            .catch((error) => {
                console.log(error);
            })
        },

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
            db.collection("tags").doc(inputID).delete().then(() => {
                console.log("Tag deleted: "+ inputID);
            }).catch((error) => {
                console.error("Error removing document: ", error);
            });
        
        },
        show () {
        this.$refs.dialog.show()
        },
        hide () {
        this.$refs.dialog.hide()
        },

        onDialogHide () {
        this.$emit('hide')
        },

        onOKClick () {
        this.$emit('ok')
        this.hide()
        },

        onCancelClick () {
        this.hide()
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

