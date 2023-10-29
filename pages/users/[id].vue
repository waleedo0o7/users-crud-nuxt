<template>
  <div class="d-flex justify-content-between align-items-center top-nav-wrapper p-2">
    <h3 class="text-capitalize">welcome to crud task</h3>
    <button @click="logout" class="btn btn-primary">Logout</button>
  </div>

  <div class="page">
    <div class="container">
      <div class="form-container">
        <div v-if="actionType === 'view'" class="view-user-page-wrapper">
          <h1 class="text-center text-capitalize">user details</h1>
          <p>
            <span class="lead text-bold"> First Name :</span>
            {{ user.firstName }}
          </p>
          <p>
            <span class="lead text-bold"> maiden Name </span>
            {{ user.maidenName }}
          </p>
          <p><span class="lead text-bold"> email </span> {{ user.email }}</p>
          <p><span class="lead text-bold"> age </span> {{ user.age }}</p>
        </div>

        <div v-if="actionType === 'edit'" class="edit-user-page-wrapper">
          <h1 class="text-center text-capitalize">edit user</h1>

          <form @submit.prevent="editUser">
            <div class="row">
              <div class="col-md-4">
                <div class="form-group">
                  <label> first name </label>
                  <input
                    type="text"
                    v-model="user.firstName"
                    class="form-control"
                    placeholder="first name"
                  />
                </div>
              </div>
              <div class="col-md-4">
                <div class="form-group">
                  <label> Last name </label>
                  <input
                    type="text"
                    v-model="user.lastName"
                    class="form-control"
                    placeholder="last name"
                  />
                </div>
              </div>

              <div class="col-md-4">
                <div class="form-group">
                  <label> maiden Name </label>
                  <input
                    type="text"
                    v-model="user.maidenName"
                    class="form-control"
                    placeholder="maiden   name"
                  />
                </div>
              </div>

              <div class="col-md-6">
                <div class="form-group">
                  <label> email </label>
                  <input
                    type="text"
                    v-model="user.email"
                    class="form-control"
                    placeholder="email    "
                  />
                </div>
              </div>

              <div class="col-md-6">
                <div class="form-group">
                  <label> age </label>
                  <input
                    type="text"
                    v-model="user.age"
                    class="form-control"
                    placeholder="age    "
                  />
                </div>
              </div>
            </div>

            <button class="btn btn-primary" type="submit">Edit</button>
          </form>
        </div>

        <div v-if="actionType === 'delete'" class="delete-user-page-wrapper">
          <h1 class="text-center text-capitalize">Delete user</h1>

          <form class="text-center" @submit.prevent="deleteUser">
            <p class="h4">
              are you sure you want to delete ' {{ user.firstName }} ' user ?
            </p>

            <button class="btn btn-danger" type="submit">Delete</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

import { useRoute } from "vue-router";

const router = useRouter();

const $route = useRoute();

const actionType = $route.query.action;

const userID = $route.params.id;

const uri = `https://dummyjson.com/users/${userID}`;

const { data: user } = await useFetch(uri, {
  onResponse({ request, response, options }) {},
  onResponseError({ request, response, options }) {
    console.log(response);
    alert(response._data.message);
  },
});

let editUser = async (user) => {
  const { data, pending, error } = await useFetch(uri, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      firstName: user.firstName,
      lastName: user.lastName,
      maidenName: user.maidenName,
    }),

    onResponse({ response, options }) {
      alert("user updated successful");
      router.push("/users");
    },

    onResponseError({ request, response, options }) {
      console.log(response);
      alert(response._data.message);
    },
  });
};

const deleteUser = async () => {
  const { data, pending, error } = await useFetch(uri, {
    method: "DELETE",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      firstName: "",
    }),

    onRequest() {},

    onResponse({ request, response, options }) {
      if (response.status === 200) {
        alert("user deleted successful");
        router.push("/users");
      }
    },

    onResponseError({ request, response, options }) {
      console.log(response);
      alert(response._data.message);
    },
  });
};

</script>

<style lang="scss" scoped></style>
