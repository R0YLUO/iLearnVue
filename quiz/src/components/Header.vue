<template>
  <b-nav tabs>
    <b-nav-item disabled><b>My Quiz App</b></b-nav-item>
    <b-nav-item disabled>Counter: {{ numCorrect }}/{{ totalAnswered }}</b-nav-item>
  </b-nav>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'totalAnswered'
  ]
}
</script>