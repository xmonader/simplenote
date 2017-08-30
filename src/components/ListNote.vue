<template>
  <div class="feed">
    <template v-if="loading > 0">
        Loading...
</template>

<template v-else>
  <ul>
    <li v-for="note in allNotes" :key="note.id">
      <note :note='note' class="note" />
    </li>
  </ul>
</template>
  </div>
</template>

<style>

  .note {
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
  
  // Component def
  export default {
    // Local state
    data: () => ({
      allNotes: {},
      loading: 0,
    }),
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
    }
  }
</script>
