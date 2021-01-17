<template>
  <!-- section main -->
  <section class="main" id="main">
    <div class="row" id="menu-list">
      <div v-for="(element, index) in listItems" :key="index">
        <div class="col-sm-6 col-md-4">
          <div class="card mb-5 ml-n2 mr-n2">
            <img
              :src="element.image"
              class="card-img-top"
              alt="image"
              id="img-cursor"
            />
            <div class="card-body">
              <h5 class="card-title">{{ element.name }}</h5>
              <span class="prices">{{ convert(element.price) }}</span>
              <button
                @click="readDetail(element.id)"
                class="btn btn-primary bt-detail"
              >
                Detail <i class="fas fa-info-circle"></i>
              </button>
              <button
                @click="addCart(element)"
                class="btn btn-success ml-3 bt-addItem"
              >
                Add <i class="fas fa-cart-plus"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
  <!-- section main end -->
</template>

<script>
import Axios from 'axios'
export default {
  data () {
    return {
      listItems: []
    }
  },
  methods: {
    convert: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
    },
    readDetail (id) {
      this.$router.push({ path: '/detail/' + id })
    },
    getdataProduct () {
      // const data = '?name=tea'
      // const data = '?page=1&limit=8'
      // const data = '?order=name&metode=asc'
      // const data = 'code=Coffee'
      Axios.get('http://localhost:3000/product').then((response) => {
        this.listItems = response.data
      }).catch((err) => {
        console.log(err)
      })
    },
    addCart (data) {
      this.$emit('thisEmit', data)
      // console.log(data)
    }
  },
  mounted () {
    this.getdataProduct()
  }
}
</script>
