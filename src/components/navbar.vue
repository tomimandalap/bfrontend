<template>
  <div>
    <nav class="navbar navbar-expand-lg mt-n1">
      <div class="container-fluid justify-content-center">
        <div v-if="status === true">
          <button @click="hideBar()" class="btn btn-light">
            <i class="fas fa-bars"></i>
          </button>
        </div>
        <div v-else>
          <button @click="showBar()" class="btn btn-light">
            <i class="fas fa-bars"></i>
          </button>
        </div>
      </div>
      <div class="container justify-content-start">
        <router-link to="/" class="nav-link">{{ titleText }}</router-link>
      </div>
      <div class="container justify-content-center">
        <button class="btn btn-light search" id="search-btn" type="button">
          <i class="fas fa-search"></i>
        </button>
      </div>

      <div class="container justify-content-end">
        <div v-if="state === true">
          <button
            @click="hideRIGHT()"
            class="btn btn-light cart"
            id="cart-btn"
            type="button"
          >
            <p>{{ txtCart }}</p>
          </button>
        </div>
        <div v-else>
          <button
            @click="showRIGHT()"
            class="btn btn-light cart"
            id="cart-btn"
            type="button"
          >
            <p>{{ txtCart }}</p>
          </button>
        </div>
        <span class="index-item" id="index-item">{{ txtItems }}</span>
      </div>
    </nav>
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'Header',
  data () {
    return {
      txtCart: 'Cart',
      txtItems: 0,
      status: true,
      state: true
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
    },
    hideRIGHT () {
      document.getElementById('side-right').classList.add('active')
      this.state = false
    },
    showRIGHT () {
      document.getElementById('side-right').classList.remove('active')
      this.state = true
    }
  }
}
</script>
<style src='../assets/css/styles.css'></style>
