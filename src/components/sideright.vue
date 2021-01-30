<template>
  <div class="container-bars">
    <contentLeft />
    <contenView v-on:thisEmit="actionADD" />
    <div v-if="stateShow">
      <aside class="right" id="side-right">
        <div class="containter-cart">
          <div v-for="(items, index) in dataCART" :key="index">
            <div class="row">
              <div class="col-4 mt-2">
                <img :src="items.image" :alt="items.name" id="img-cart" />
              </div>
              <div class="col-4">
                <h3 class="title-cart">{{ items.name }}</h3>
                <button
                  @click="btnDecrement(items.id)"
                  class="btn btn-success btn-min"
                >
                  <i class="fas fa-minus"></i>
                </button>
                <span class="cart-items">x{{ items.qty }}</span>
                <button
                  @click="btnIncrement(items.id)"
                  class="btn btn-success btn-plus"
                >
                  <i class="fas fa-plus"></i>
                </button>
              </div>
              <div class="col-4">
                <button
                  @click="deleteITEM(items.id)"
                  class="btn btn-outline-danger"
                  id="deleteCart"
                >
                  <i class="fas fa-times"></i>
                </button>
                <span class="total-items">{{ convertRP(items.subtotal) }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="row" id="line-cart">
          <div class="col-6">
            <h3 class="buy-total ml-2">Total</h3>
          </div>
          <div class="col-6">
            <span class="total-price">{{ convert(total) }}</span>
          </div>
        </div>
        <div class="row">
          <div class="col-12 ml-2">
            <p class="no-ppn">*Belum termasuk ppn</p>
          </div>
        </div>
        <div class="form-group row">
          <label for="invoices-form">Invoice</label>
          <div class="input">
            <input
              type="number"
              v-model="formdata.invoices"
              class="form-control"
              placeholder="1234"
              id="invoices-form"
            />
          </div>
        </div>
        <div class="form-group row">
          <label for="cashier-form">Cashier</label>
          <div class="input">
            <select id="cashier-form" v-model="formdata.nameCashier">
              <option value="">Select Name</option>
              <option value="Cashier 1">Cashier 1</option>
              <option value="Cashier 2">Cashier 2</option>
              <option value="Pevita Pearce">Pevita Pearce</option>
            </select>
          </div>
        </div>
        <div class="row">
          <div class="col-12">
            <button
              @click="checkOUT()"
              class="btn btn-primary"
              id="btn-checkout"
            >
              Checkout
            </button>
          </div>
        </div>
        <div class="row">
          <div class="col-12">
            <button
              @click="cancelCart()"
              class="btn btn-danger"
              id="btn-cancel-cart"
            >
              Cancel
            </button>
          </div>
        </div>
      </aside>
      <div class="modalcheckout">
        <b-modal
          ref="my-modal-checkout"
          hide-footer
          :title="'Checkout ' + 'code pemesanan #' + formdata.invoices"
        >
          <div class="row mb-3">
            <div class="col-12">
              <h6>Cashier: {{ formdata.nameCashier }}</h6>
            </div>
          </div>
          <div
            class="d-block text-left breakpoint"
            v-for="(items, index) in dataCART"
            :key="index"
          >
            <div class="row">
              <div class="col-6 mt-n3">{{ items.name }}</div>
              <div class="col-2 mt-n3">x {{ items.qty }}</div>
              <div class="col-4 mt-n3">
                <p class="p-total">
                  {{ convertRP(items.price * items.qty) }}
                </p>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col-8">
              <p class="text-ppn">Ppn 10%</p>
            </div>
            <div class="col-4">
              <span class="total-check-items">{{
                convertRP(0.1 * total)
              }}</span>
            </div>
          </div>
          <div class="row mt-n3">
            <div class="col-12">
              <p class="p-total">
                Total:
                <span class="total-pay">{{
                  convertRP(0.1 * total + total)
                }}</span>
              </p>
            </div>
            <div class="col-12">
              <p class="payment">Payment: Cash</p>
            </div>
          </div>
          <div class="d-block text-center">
            <b-button class="mt-3 mb-3" variant="primary" block @click="print()"
              >Send Data</b-button
            >
            <h6>Or</h6>
            <b-button class="mt-3" variant="danger" block @click="sendEmail()"
              >Send Email</b-button
            >
            <b-button
              class="mt-3 mb-3"
              variant="dark"
              block
              @click="cancelModel()"
              >Cancel</b-button
            >
          </div>
        </b-modal>
      </div>
    </div>
    <div v-else>
      <aside class="right" id="side-right">
        <div class="containter-cart">
          <img
            class="image"
            src="../assets/img/food-and-restaurant.png"
            alt="image-empity"
          />
          <h3 class="menu-cart" id="menu_cart">Your cart is empty</h3>
          <p class="menu-add"><i>Please add some items from the menu!</i></p>
        </div>
      </aside>
    </div>
  </div>
</template>
<script>
import contentLeft from '../components/sideleft'
import contenView from '../components/content'
import Axios from 'axios'
export default {
  data () {
    return {
      stateShow: false,
      dataCART: [],
      datalist: [],
      formdata: {
        invoices: '',
        nameCashier: ''
      },
      total: 0,
      idx: 0
    }
  },
  components: {
    contentLeft,
    contenView
  },
  methods: {
    convertRP: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
    },
    convert: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', ',- *').replace('Rp', 'Rp.')
    },
    deleteITEM (id) {
      const deletenewData = this.dataCART.filter((item) => {
        return item.id !== id
      })
      this.dataCART = deletenewData
      if (this.dataCART.length === 0) {
        this.stateShow = false
        this.dataCART = []
        this.idx = 0
        document.querySelector('span.index-item').textContent = this.idx
      }
      this.totalPrice()
      this.indexCart()
    },
    totalPrice () {
      this.total = 0
      this.dataCART.forEach((elm) => {
        this.total = this.total + elm.subtotal
      })
    },
    indexCart () {
      this.idx = 0
      this.dataCART.forEach((elm) => {
        this.idx = this.idx + elm.qty
        document.querySelector('span.index-item').textContent = this.idx
      })
    },
    actionADD (data) {
      // console.log(data)
      this.stateShow = true
      const checkData = this.dataCART.filter((item) => {
        return item.id === data.id
      })
      if (checkData.length >= 1) {
        alert('Data sudah ada di list cart!')
        // this.dataCART.forEach(element => {
        //   if (element.id === data.id) {
        //     element.qty += 1
        //   }
        // })
      } else {
        const newDataCart = {
          qty: 1,
          // invoices: this.formdata.invoices,
          id: data.id,
          name: data.name,
          // cashier: this.formdata.nameCashier,
          category: data.category,
          price: data.price,
          image: data.image,
          subtotal: 1 * data.price
        }
        // this.dataCART = [...this.dataCART, data]
        this.dataCART = [...this.dataCART, newDataCart]
        // document.querySelector('span.index-item').textContent = 1
      }
      this.totalPrice()
      this.indexCart()
    },
    btnDecrement (id) {
      const min = 1
      this.dataCART.forEach(element => {
        if (element.id === id) {
          element.qty -= 1
          element.subtotal = element.qty * element.price
          if (element.qty < min) {
            alert('Minimum Pemesanan' + min + ' item!')
            element.qty = min
            element.subtotal = min * element.price
          }
        }
      })
      this.totalPrice()
      this.indexCart()
    },
    btnIncrement (id) {
      const max = 100
      this.dataCART.forEach(element => {
        if (element.id === id) {
          element.qty += 1
          element.subtotal = element.qty * element.price
          if (element.qty > max) {
            alert('Maksimum pemesanan ' + max + ' item!')
            element.qty = max
            element.subtotal = max * element.price
          }
        }
      })
      this.totalPrice()
      this.indexCart()
    },
    checkOUT () {
      this.$refs['my-modal-checkout'].show()
      this.dataCART.forEach((e) => {
        this.datalist.push({
          id: e.id,
          invoices: this.formdata.invoices,
          cashier: this.formdata.nameCashier,
          item: e.name,
          qty: e.qty,
          price: e.price,
          subtotal: e.subtotal
        })
      })
      // console.log(this.datalist)
      // send data to table cart
      Axios.post('http://localhost:3000/cart', this.datalist).then((response) => {
        console.log(response)
      }).catch((error) => {
        console.log(error)
      })
    },
    cancelModel () {
      this.$refs['my-modal-checkout'].hide()
      // console.log(this.dataCART[0].invoices)
      // delete data tabel cart base on number invoice
      Axios.delete('http://localhost:3000/cart/' + this.dataCART[0].invoices).then((response) => {
        console.log(response)
      }).catch((error) => {
        console.log(error)
      })
    },
    print () {
      // alert('print data!')
      const newDATA = []
      this.datalist.forEach((e) => {
        newDATA.push({
          invoices: e.invoices,
          cashier: e.cashier,
          cart: e.item + ' x' + e.qty
        })
      })

      // console.log(newDATA)

      // grab invoices data
      const idorder = newDATA[0].invoices
      const invoices = '#' + idorder.toString()
      // grab cashier data
      const cashier = newDATA[0].cashier
      // grab date data
      const date = '18 Januari 2020'
      // grab cart data
      const mapping = newDATA.map((el) => {
        return el.cart
      })
      const cart = mapping.join(', ')
      // grab total
      const ppn = 0.1 // 10%
      const amount = ppn * this.total + this.total

      // output
      // console.log(invoices)
      // console.log(cashier)
      // console.log(date)
      // console.log(cart)
      // console.log(amount)

      // object data
      const history = {
        invoices,
        cashier,
        date,
        cart,
        amount
      }
      // output data object
      // console.log(history)

      // send data
      Axios.post('http://localhost:3000/history', history).then((response) => {
        // this.$router.push({ path: '/' })
        this.$refs['my-modal-checkout'].hide()
        this.cancelCart()
      }).catch((err) => {
        console.log(err)
      })
    },
    sendEmail () {
      alert('send to email')
      this.cancelCart()
    },
    cancelCart () {
      this.stateShow = false
      this.dataCART = []
      this.datalist = []
      this.idx = 0
      document.querySelector('span.index-item').textContent = this.idx
    }
  }
}
</script>
