<template>
  <div id="credits-content">
    <ul> Credits:
      <li v-for="user in credits">
        <div>
          {{ user.firstName }} {{ user.lastName }}
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'credits',
    data () {
      return {
        credits: [
          {firstName: 'Alexey', lastName: 'Romanov'}
        ]
      }
    }
  }
</script>

<style scoped>


  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  #credits-content {
    position: absolute;
    bottom: 0;
    margin: 0 auto;
    width: 10em;
  }

</style>
