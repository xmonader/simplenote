<template>
  <div class='container'>
  <div><h1>{{ note.title }}</h1>
</div>
    <br />
    <div>{{ note.content }} </div>
  </div>
</template>

<script>
  export default {
    data: () => ({

    }),
    props: {
      note: {
        type: Object,
        default: function () {
          return {id:'', title: '', content: '' }
        }
      },
    },

  }
</script>
<style>
/* Add some padding inside the card container */
.container {
    padding: 2px 16px;
    margin:10px;
}
</style>
