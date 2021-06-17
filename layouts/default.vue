<template>
  <div>
    <navbar />
    <main>
      <div class="container">
        <Nuxt />
      </div>
    </main>
  </div>
</template>

<style>

</style>

<script>
import Navbar from "~/components/navbar";
export default {
  components: {Navbar}
}
</script>
