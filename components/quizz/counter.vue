<template>
<div class="container flex justify-center content-center flex-col mx-auto text-center">
  <section class="flex text-3xl justify-center content-center flex-col mx-auto text-center"></section>
  <section class="flex text-10xl justify-center content-center row">
    <div class="minutes mx-2 relative">
      {{ displayMinutes }}
      <div class="label text-sm absolute bottom-0">минут</div>
    </div>
    <span class="leading-snug">:</span>
    <div class="minutes ml-2 relative">
      {{ displaySeconds }}
      <div class="label text-sm absolute bottom-0">секунд</div>
    </div>
  </section>
</div>
</template>

<script>
export default {
  name: "counter",
  data: () => ({
    displayMinutes: 0,
    displaySeconds: 0,
  }),

  computed: {
    _seconds: () => 1000,
    _minutes()  {
      return this._seconds * 60
    },
    _hours() {
      return this._minutes * 60
    }
  },

  mounted() {
    this.showRemaining()
  },

  methods: {
    formatNum: num => (num < 10 ? "0" + num : num),

    showRemaining() {
      const now = new Date()
      const end = new Date(now.getFullYear(), now.getMonth(), now.getDate(), now.getHours(), now.getMinutes() + 10, now.getSeconds(), now.getMilliseconds())

      const timer = setInterval(() => {
        const start = new Date()
        const distance = end.getTime() - start.getTime()

        if (distance <= 0) {
          clearInterval(timer)
          alert('Время вышло')
          return
        }

        const minutes = Math.floor((distance % this._hours) / this._minutes)
        const seconds = Math.floor((distance % this._minutes) / this._seconds)

        this.displayMinutes = this.formatNum(minutes)
        this.displaySeconds = this.formatNum(seconds)
      }, 1000)
    }
  }
}
</script>

<style scoped>

</style>
