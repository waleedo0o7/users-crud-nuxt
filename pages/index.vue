<template>
  <div></div>
</template>

<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

router.push("/auth/login");
</script>

<style lang="scss">

@import url("https://cdnjs.cloudflare.com/ajax/libs/bootstrap/4.6.2/css/bootstrap.min.css");

.page {
  background: rgb(244, 244, 244);
  min-height: 100vh;
  padding: 30px;
}

.table {
  background: #fff;
  td,
  th {
    padding: 6px;
  }
}

.form-wrapper {
  width: 400px;
  max-width: 100%;
}

.form-control {
  color: rgb(0, 0, 0);
}

.text-bold {
  font-weight: bold;
}

.form-container {
    background: #FFF;
    padding: 30px;
    box-shadow: 0 0 5px #e9e9e9;
}

</style>
