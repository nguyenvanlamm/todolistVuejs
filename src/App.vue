<template>
  <div>
    <span>{{ tongtien | danhDau }}</span>
    <button @click="tongtien+=20000">Cong tien</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      tongtien: 100000
    }
  },
  filters: {
    danhDau: function (tongtien) {
      return tongtien.toFixed(2).replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,")
    }
  }
}

</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
