<template>
  <div class="card card-user">
    <div class="image">
      <img src="/static/img/background/damir-bosnjak.jpg" alt="..." />
    </div>
    <div class="card-body">
      <div class="author">
        <a href="#">
          <img
            class="avatar border-gray"
            src="/static/img/mike.jpg"
            alt="..."
          />
          <h5 class="title">{{ user.full_name }}</h5>
        </a>
        <p class="description">
          {{ user.full_name }}
        </p>
      </div>
      <p class="description text-center">
        <br />
        {{ user.phone_number }} <br />
        {{ user.email }} <br />
        Address
      </p>
    </div>
    <div class="card-footer">
      <hr />
      <div class="button-container">
        <div class="row">
          <div class="col-lg-3 col-md-6 col-6 ml-auto">
            <h5>
              <br />
              <small></small>
            </h5>
          </div>
          <div class="col-lg-4 col-md-6 col-6 ml-auto mr-auto">
            <h5>
              <br />
              <small></small>
            </h5>
          </div>
          <div class="col-lg-3 mr-auto">
            <h5>
              <br />
              <small></small>
            </h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["user"],
  data() {
    return {
      user: {...this.user},
    };
  },
};
</script>
<style>
</style>
