<template>
  <div>
    <!-- aside left -->
    <aside class="left" id="side-left">
      <ul class="list-bars">
        <li>
          <router-link to="/" class="icon-list"
            ><i class="fas fa-utensils"></i
          ></router-link>
        </li>
        <li>
          <router-link to="/history" class="icon-list"
            ><i class="far fa-clipboard"></i
          ></router-link>
        </li>
        <li>
          <button
            @click="addModals()"
            class="btn btn-light bars-item3"
            id="btn-plus"
            type="button"
          >
            <i class="fas fa-plus"></i>
          </button>
        </li>
      </ul>
    </aside>
    <!-- aside left end-->
    <div>
      <b-modal ref="my-modal-plus" hide-footer title="Add Item" class="addALL">
        <div class="d-block text-left breakadditems">
          <div class="row">
            <div class="col-2">
              <label for="name-form">Name</label>
            </div>
            <div class="col-10">
              <input
                type="text"
                v-model="form.name"
                class="form-control mb-2"
                placeholder="Name"
                id="name-form"
              />
            </div>
          </div>
          <div class="row">
            <div class="col-2"><label for="image-form">Image</label></div>
            <div class="col-10">
              <input
                type="text"
                v-model="form.image"
                class="form-control mb-2"
                placeholder="Url image"
                id="image-form"
              />
            </div>
          </div>
          <div class="row">
            <div class="col-2"><label for="price-form">Price</label></div>
            <div class="col-10">
              <input
                type="text"
                v-model="form.price"
                class="form-control mb-2"
                placeholder="Price"
                id="price-form"
              />
            </div>
          </div>
          <div class="row">
            <div class="col-2"><label for="category-form">Category</label></div>
            <div class="col-10">
              <div class="input">
                <select id="category-form" v-model="form.category">
                  <option value="">Category</option>
                  <option
                    v-for="(element, index) in listcategory"
                    :key="index"
                    :value="element.category"
                  >
                    {{ element.category }}
                  </option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <b-button class="mt-3" variant="primary" block @click="submitData()"
          >Add</b-button
        >
        <b-button class="mt-3" variant="danger" block @click="hideModalPlus()"
          >Cancel</b-button
        >
      </b-modal>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
// import router from '../router/index'
export default {
  data () {
    return {
      state: false,
      form: {
        category: '',
        name: '',
        image: '',
        price: ''
      },
      listcategory: []
    }
  },
  methods: {
    addModals () {
      this.$refs['my-modal-plus'].show()
      Axios.get('http://localhost:3000/category').then((response) => {
        this.listcategory = response.data.data
        // console.log(response.data.data)
      }).catch((error) => {
        console.log(error)
      })
    },
    hideModalPlus () {
      this.$refs['my-modal-plus'].hide()
    },
    submitData () {
      Axios.post('http://localhost:3000/product', this.form).then((response) => {
        // console.log(this.form)
        // this.$router.push({ path: '/' })
        this.$refs['my-modal-plus'].hide()
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>
