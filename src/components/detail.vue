<template>
  <div class="container mt-3">
    <h1 style="text-align: left">Detail Item {{ id }}</h1>
    <div v-if="isLoading">
      <h1>Loading Data</h1>
    </div>
    <div v-else>
      <div class="row mt-3">
        <div class="col-sm-12 col-md-12 col-lg-3 mb-3">
          <img
            class="img-details"
            :src="resultItem[0].image"
            :alt="resultItem[0].name"
          />
        </div>
        <div class="col-sm-12 col-md-12 col-lg-9">
          <table class="table mt-1">
            <thead class="thead-dark">
              <tr>
                <th scope="col">Category</th>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
                <th scope="col">Date</th>
              </tr>
            </thead>
            <tbody style="border-bottom: 2px solid #000">
              <tr>
                <th scope="row">{{ resultItem[0].category }}</th>
                <td>{{ resultItem[0].name }}</td>
                <td>{{ convert(resultItem[0].price) }}</td>
                <td>{{ resultItem[0].date.substring(0, 10) }}</td>
              </tr>
            </tbody>
          </table>
          <button @click="back()" class="btn btn-dark btn-detail">
            <i class="fas fa-angle-double-left"></i> Back
          </button>
          <button @click="update()" class="btn btn-success ml-3 btn-detail">
            <i class="far fa-edit"></i> Update
          </button>
          <button @click="deleteRow()" class="btn btn-danger ml-3 btn-detail">
            <i class="far fa-trash-alt"></i> Delete
          </button>
        </div>
      </div>
    </div>
    <div class="modal-updateItem">
      <b-modal ref="my-modal" hide-footer :title="'Update data [' + id + ']'">
        <div
          class="d-block text-center"
          v-for="(element, index) in resultItem"
          :key="index"
        >
          <input
            type="text"
            v-model="fUpdate.name"
            class="form-control mb-2"
            :placeholder="element.name"
          />
          <input
            type="text"
            v-model="fUpdate.image"
            class="form-control mb-2"
            :placeholder="element.image"
          />
          <input
            type="text"
            v-model="fUpdate.price"
            class="form-control mb-2"
            :placeholder="element.price"
          />
          <input
            type="text"
            v-model="fUpdate.category"
            class="form-control mb-2"
            :placeholder="element.category"
          />
        </div>
        <b-button class="mt-3" variant="primary" block @click="updateRow()"
          >Update Data</b-button
        >
        <b-button class="mt-3" variant="danger" block @click="hideModal"
          >Cancel</b-button
        >
      </b-modal>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
export default {
  data () {
    return {
      id: this.$route.params.id,
      isLoading: true,
      resultItem: {},
      fUpdate: {
        category: '',
        name: '',
        image: '',
        price: ''
      }
    }
  },
  methods: {
    convert: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
    },
    getDetailItem () {
      Axios.get('http://localhost:3000/product/' + this.id).then((response) => {
        this.resultItem = response.data.data
        // console.log(response.data.data)
      }).catch((err) => {
        console.log(err)
      }).finally(() => {
        this.isLoading = false
      })
    },
    back () {
      this.$router.push({ path: '/' })
    },
    update () {
      this.$refs['my-modal'].show()
    },
    hideModal () {
      this.$refs['my-modal'].hide()
    },
    updateRow () {
      Axios.put('http://localhost:3000/product/' + this.id, this.fUpdate).then((response) => {
        // console.log(this.fUpdate)
        this.$router.push({ path: '/' })
      }).catch((err) => {
        console.log(err)
      })
    },
    deleteRow () {
      Axios.delete('http://localhost:3000/product/' + this.id).then((response) => {
        this.$router.push({ path: '/' })
      }).catch((err) => {
        console.log(err)
      })
    }
  },
  mounted () {
    this.getDetailItem()
  }
}
</script>
<style scoped>
img.img-details {
  width: 225px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
</style>
