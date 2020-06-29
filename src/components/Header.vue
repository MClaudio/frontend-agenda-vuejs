<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <router-link :to="{ name: 'Home' }">
      <a class="navbar-brand">Agenda API</a>
    </router-link>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarText"
      aria-controls="navbarText"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarText">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <router-link :to="{ name: 'Home' }">
            <a class="nav-link">Home <span class="sr-only">(current)</span></a>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link :to="{ name: 'About' }">
            <a class="nav-link">Acerca de</a>
          </router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Header",
  props: {
    msg: String,
  },
};
</script>
