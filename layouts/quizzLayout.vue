<template>
  <div class="quizz">
    <navbar />
    <div class="container border rounded"
         style="background-color: #f7f7f9;
         border-bottom: 1px solid #a8a8a8; border-right: 1px solid #a8a8a8; border-left: 1px solid #a8a8a8">
      <nuxt />
    </div>
  </div>
</template>

<script>
import Navbar from "~/components/navbar";
export default {
  components: {Navbar}
}
</script>

<style scoped>

</style>
