<template>
  
  <div class="d-flex justify-content-between align-items-center top-nav-wrapper p-2">
    <h3 class="text-capitalize"> welcome to crud task</h3>
    <button @click="logout" class="btn btn-primary"> Logout </button>
  </div>

  <div class="page users-page">
    <div class="container">

      <h1 class="mb-3 text-center">Users List</h1>

      <table class="table">
        <thead class="thead-dark">
          <tr>
            <th>#</th>
            <th>firstName</th>
            <th>lastName</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users.users">
            <th>{{ user.id }}</th>
            <th>{{ user.firstName }}</th>
            <th>{{ user.lastName }}</th>
            <th>
              <div class="actions">

                <NuxtLink :to="`/users/${user.id}?action=view`" class="btn btn-primary mr-2"> View </NuxtLink>

                <NuxtLink :to="`/users/${user.id}?action=edit`" class="btn btn-success mr-2"> Edit </NuxtLink>

                <NuxtLink :to="`/users/${user.id}?action=delete`" class="btn btn-danger mr-2"> Delete </NuxtLink>

              </div>
            </th>
          </tr>
        </tbody>
      </table>

    </div>
  </div>
</template>

<script setup>

const router = useRouter();

const { data: users } = await useFetch("https://dummyjson.com/users", {

  onResponse({ request, response, options }) {
    if (response.status === 200) {
      // console.log(response._data.users); 
    }
  },

  onResponseError({ request, response, options }) {
    console.log(response);
    alert(response._data.message);
  },
});


const logout = () => {
  localStorage.removeItem('token')
    router.push("/auth/login");
}

onMounted(() => {
  if (!localStorage.getItem('token')) {
    router.push("/auth/login");
  }
});

</script>

<style lang="scss"></style>
