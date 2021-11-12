<template>
  <div v-if="user" class="row">
    <div class="col-lg-7 col-md-7">
      <user-card :user="user" />
    </div>
    <div class="col-lg-5 col-md-5">
      <edit-profile-card :user="user" />
      <edit-password-card :user="user" />
    </div>
  </div>
</template>
<script>
import EditProfileCard from "./UserProfile/EditProfileCard.vue";
import UserCard from "../Pages/UserProfile/UserCard.vue";
import EditPasswordCard from "./UserProfile/EditPasswordCard.vue";
import { GET_USER } from "../../../../constants/api";
import axios from "axios";

export default {
  components: {
    EditProfileCard,
    UserCard,
    EditPasswordCard,
  },

  data: () => ({
    user:{},
    token: localStorage.token || "",
  }),

  created() {
    this.getProfile();
    console.log(this.token);
  },

  methods: {
    getProfile() {
      axios.defaults.headers.common["Authorization"] = `Bearer ${this.token}`;
      axios
        .get(`${GET_USER}`)
        .then((res) => {
          // console.log(res);
          this.user = res.data.data;
        })
        .catch((err) => {
          console.log("error" + err);
        });
    },
  },
};
</script>

<style>
</style>
