<template>
  <div>
    <nav class="navbar navbar-expand-lg mt-n1">
      <div class="container-fluid justify-content-center">
        <div v-if="status === true">
          <button v-on:click="hideBar()" class="btn btn-light">
            <i class="fas fa-bars"></i>
          </button>
        </div>
        <div v-else>
          <button v-on:click="showBar()" class="btn btn-light">
            <i class="fas fa-bars"></i>
          </button>
        </div>
      </div>
      <div class="container justify-content-start">
        <router-link to="/history" class="nav-link">{{
          titleText
        }}</router-link>
      </div>
    </nav>
    <router-view />
  </div>
</template>

<script>
export default {
  data () {
    return {
      status: true
    }
  },
  props: {
    titleText: String
  },
  methods: {
    hideBar: function () {
      document.getElementById('side-left').classList.add('active')
      document.getElementById('main').classList.add('setting')
      this.status = false
    },
    showBar: function () {
      document.getElementById('side-left').classList.remove('active')
      document.getElementById('main').classList.remove('setting')
      this.status = true
    }
  }
}
</script>
