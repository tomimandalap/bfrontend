b<template>
  <!-- section main -->
  <section class="main" id="main">
    <!-- history-card -->
    <div class="row" id="history-card">
      <div class="col-12 col-md-6 col-lg-4">
        <div class="cards today">
          <div class="dot1 dot-up"></div>
          <div class="dot1 dot-center"></div>
          <div class="dot1 dot-down"></div>
          <div class="cards-body">
            <h5 class="cards-title">Today’s Income</h5>
            <h3 class="cards-nominal">
              Rp. <span class="no-today">1.000.000</span>
            </h3>
            <p class="cards-text">+2% Yesterday</p>
          </div>
        </div>
      </div>
      <div class="col-12 col-md-6 col-lg-4">
        <div class="cards orders">
          <div class="dot2 dot-up"></div>
          <div class="dot2 dot-center"></div>
          <div class="dot2 dot-down"></div>
          <div class="cards-body">
            <h5 class="cards-title">Orders</h5>
            <h3 class="cards-nominal">
              Rp. <span class="no-today">3.270</span>
            </h3>
            <p class="cards-text">+5% Last Week</p>
          </div>
        </div>
      </div>

      <div class="col-12 col-md-6 col-lg-4">
        <div class="cards years">
          <div class="dot3 dot-up"></div>
          <div class="dot3 dot-center"></div>
          <div class="dot3 dot-down"></div>
          <div class="cards-body">
            <h5 class="cards-title">This Year’s Income</h5>
            <h3 class="cards-nominal">
              Rp. <span class="no-today">100.000.000.000</span>
            </h3>
            <p class="cards-text">+10% Last Year</p>
          </div>
        </div>
      </div>
    </div>
    <!-- history-card end -->

    <!-- box revenue -->
    <div class="row justify-content-center" id="history-graph">
      <div class="col-12 col-md-12">
        <div class="revenue-box">
          <p><b>Revenue</b></p>

          <div class="dropdown">
            <a
              class="btn btn-secondary dropdown-toggle"
              href=""
              role="button"
              id="dropdownMenuLink"
              data-toggle="dropdown"
              aria-haspopup="true"
              aria-expanded="false"
            >
              Month
            </a>

            <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
              <a class="dropdown-item" href="#">Day</a>
              <a class="dropdown-item" href="#">Month</a>
              <a class="dropdown-item" href="#">Year</a>
            </div>
          </div>

          <div class="chart-container">
            <!-- <canvas id="revenueChart"><graph /></canvas> -->
          </div>
        </div>
      </div>
    </div>
    <!-- box revenue end -->
    <!-- box recent order -->
    <div class="row" id="history-order">
      <div class="col-12 col-md-12">
        <div class="recent-box">
          <p><b>Recent Order</b></p>

          <div class="dropdown">
            <a
              class="btn btn-secondary dropdown-toggle"
              href=""
              role="button"
              id="dropdownMenuLink"
              data-toggle="dropdown"
              aria-haspopup="true"
              aria-expanded="false"
            >
              Today
            </a>

            <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
              <a class="dropdown-item" href="#">Today</a>
              <a class="dropdown-item" href="#">Months</a>
              <a class="dropdown-item" href="#">Years</a>
            </div>
          </div>
          <!-- pagination -->
          <!-- <div class="overflow-auto mr-4">
            <b-pagination
              @input="pageProduct('')"
              v-model="currentPage"
              :total-rows="rows"
              :per-page="perPage"
              aria-controls="my-table"
              size="sm"
              align="left"
            ></b-pagination>
          </div> -->
          <!-- tables
          <table class="tables" id="my-table">
            <thead>
              <tr>
                <th>Invoices</th>
                <th>Cashier</th>
                <th>Date</th>
                <th>Order</th>
                <th>Amount</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody
              class="table-list"
              id="my-table"
            >
              <tr>
                <td>{{ element.id }}</td>
                <td>{{ element.cashier }}</td>
                <td>{{ element.date }}</td>
                <td>{{ element.cart }}</td>
                <td>{{ convert(element.amount) }}</td>
                <td>
                  <button class="btn btn-warning">Detail</button>
                </td>
              </tr>
            </tbody>
          </table> -->
          <div class="overflow-auto mt-n4">
            <b-pagination
              @input="pageProduct('')"
              v-model="currentPage"
              :total-rows="rowData"
              :per-page="perPage"
              aria-controls="my-table"
              size="sm"
              align="left"
            ></b-pagination>

            <p class="mt-3" style="font-size: 14px">
              Current Page: {{ currentPage }}
            </p>

            <b-table
              id="my-table"
              :items="dataTabel"
              :per-page="perPage"
              :current-page="currentPage"
              head-variant="dark"
              small
              style="font-size: 14px"
            >
            </b-table>
          </div>
        </div>
      </div>
    </div>
    <!-- box recent order end -->
  </section>
</template>
<script>
import Axios from 'axios'
export default {
  data () {
    return {
      dataTabel: [],
      myData: [],
      perPage: 6,
      currentPage: 1,
      rowData: ''
    }
  },
  methods: {
    convert: (number) => {
      return number.toLocaleString('id', { style: 'currency', currency: 'IDR' }).replace(',00', '').replace('Rp', 'Rp.')
    },
    getDataTabel () {
      Axios.get('http://localhost:3000/history').then((response) => {
        this.myData = response.data.data
        this.rowData = response.data.pagination.total
        // console.log(this.myData)
        this.myData.forEach((e) => {
          this.dataTabel.push({
            id: e.id,
            invoices: e.invoices,
            cashier: e.cashier,
            date: e.date.substring(0, 10),
            cart: e.cart,
            amount: this.convert(e.amount),
            action: 'button'
          })
        })
      }).catch((err) => {
        console.log(err)
      })
    },
    pageProduct () {
      // this.getDataTabel()
      Axios.get(`http://localhost:3000/history?page=${this.currentPage}&limit=${this.perPage}`).then((response) => {
        // this.myData = response.data.data
        this.rowData = response.data.pagination.total
      }).catch((err) => {
        console.log(err)
      })
    }
  },
  mounted () {
    this.getDataTabel()
    this.pageProduct()
  }
  // computed: {
  //   rows () {
  //     return this.rowData
  //   }
  // }
}
</script>
