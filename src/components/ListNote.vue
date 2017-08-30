<template>
  <div class="feed">
    <template v-if="loading > 0">
        Loading...
</template>

<template v-else>
  <ul>
    <li v-for="(note, idx) in allNotes" :key="note.id"> 
      <div class="notecard">
        <note :note='note' class="note" /><button v-on:click="removeNote(note, idx)">X</button>
      </div>
    </li>
  </ul>
</template>
  </div>
</template>

<style>

  .notecard {
    /* Add shadows to create the "card" effect */
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    transition: 0.3s;
    width: 300px;
    height: 300px;
    float: left;
  }
  
  ul {
    list-style: none outside none;
  }
  
  li {
    position: relative;
    display: inline;
  }
  
  
  /* On mouse-over, add a deeper shadow */
  
  .post:hover {
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  }
</style>

<script>
  import gql from 'graphql-tag'
  import Note from './Note.vue'
  
  // GraphQL query
  const FeedQuery = gql `
    query allNotes {
      allNotes(orderBy: createdAt_DESC) {
        id
        title
        content 
      }
    }
  `
  const deleteNote = gql `
    mutation deleteNote($id: ID!) {
      deleteNote(id:$id) {
        id
      }
    }
  `
  // Component def
  export default {
    // Local state
    data: function(){return ({
      allNotes: {},
      loading: 0,
    }); },
    // Apollo GraphQL
    apollo: {
      allNotes: {
        query: FeedQuery,
        loadingKey: 'loading',
      },
    },
    components: {
      // <my-component> will only be available in parent's template
      'note': Note 
    },
    methods: {
      removeNote: function(note, idx) {
        this.allNotes = this.allNotes.filter((value, index, array) => idx != index)
        var id = note.id
        // Mutation
        this.$apollo.mutate({
          mutation: deleteNote,
          variables: {
            id
          },
        }).then(function(data) {
          // Result
        console.log("DONE REMOVING ...")
          console.log(data);
        }).catch((error) => {
          // Error
          console.error(error)
        })
      },
    },
  }
</script>
