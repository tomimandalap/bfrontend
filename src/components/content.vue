<template>
  <!-- section main -->
  <section class="main" id="main">
    <div class="row mb-5 search-sort">
      <div class="col-5 box-search ml-n2">
        <b-form-input
          type="text"
          v-model="search"
          @keyup="searchData(search)"
          placeholder="Search menu"
        ></b-form-input>
      </div>
      <div class="col-7 box-sort">
        <div class="container-sort">
          <select id="sort-box" v-model="formSort">
            <option value="" selected>Select</option>
            <option value="name&metode=asc">Name A-Z</option>
            <option value="name&metode=desc">Name Z-A</option>
            <option value="price&metode=asc">Price (ASC)</option>
            <option value="price&metode=desc">Price (DESC))</option>
          </select>
          <button
            @click="sortButton(formSort)"
            class="btn btn-dark btn-sort ml-2 mr-4"
          >
            Sort
          </button>
        </div>
      </div>
    </div>
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
    <div class="overflow-auto mr-4">
      <b-pagination
        @input="pageProduct('')"
        v-model="currentPage"
        :total-rows="rowData"
        :per-page="limitData"
        size="md"
        align="right"
      ></b-pagination>
    </div>
  </section>
  <!-- section main end -->
</template>

<script>
import Axios from 'axios'
export default {
  data () {
    return {
      listItems: [],
      search: '',
      formSort: '',
      icon: 'sort-alpha-down',
      status: false,
      currentPage: 1,
      rowData: '',
      limitData: 9
    }
  },
  methods: {
    convert: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
    },
    readDetail (id) {
      this.$router.push({ path: '/detail/' + id })
    },
    sortButton (datasort) {
      // const searching = this.searchData(this.search)
      this.getdataProduct([], datasort)
      // return datasort
    },
    getdataProduct (datasearch, datasort) {
      const searching = datasearch || ''
      const sorted = datasort || 'id&metode=asc'
      Axios.get(`http://localhost:3000/product?name=${searching}&order=${sorted}&page=${this.currentPage}&limit=${this.limitData}`).then((response) => {
        this.listItems = response.data.data
        this.rowData = response.data.pagination.total
        // console.log(response.data)
      }).catch((err) => {
        console.log(err)
      })
      // return [searching, sorted]
    },
    addCart (data) {
      this.$emit('thisEmit', data)
    },
    searchData (search) {
      // const datasort = this.sortButton(this.formSort)
      this.getdataProduct(search, [])
      // return search
    },
    pageProduct () {
      this.getdataProduct()
    }
  },
  mounted () {
    this.getdataProduct()
    this.pageProduct()
  }
}
</script>
