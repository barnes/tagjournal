<!-- Simple Journal Page. 
TODO: 
-Check if user is logged in. If logged in, get their collection/journal. 
-Create a simple dialog for launching a new entry component. 
-Render all previous journal entires with edit and delete options.-->

<template>
    <q-page>
        <h1>Journal Page</h1>
        <journalEntry></journalEntry>
    </q-page>
</template>

<script>
import journalEntry from '../components/journalEntry'
export default {
    components:{
        journalEntry
    }
}
</script>

