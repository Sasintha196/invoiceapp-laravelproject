<template>

    <div class="container">
        <!-- INVOICE LIST---------------------------------------- -->
        <div class="invoices">
            <div class="card__header">
                <div>
                    <h2 class="invoice__title">Invoices List</h2>
                </div>
                <div>
                    <a class="btn btn-secondary" @click="newInvoice">
                        Create New Invoice
                    </a>
                </div>
            </div>

            <div class="table card__content">
                <div class="table--filter">
                    <span class="table--filter--collapseBtn ">
                        <i class="fas fa-ellipsis-h"></i>
                    </span>
                    <div class="table--filter--listWrapper">
                        <ul class="table--filter--list">
                            <li>
                                <p class="table--filter--link table--filter--link--active">
                                    All
                                </p>
                            </li>
                            <!--
                            <li>
                                <p class="table--filter--link ">
                                    Paid
                                </p>
                            </li>
                            -->
                        </ul>
                    </div>
                </div>

                <div class="table--search">

                    <div class="table--search--wrapper">
                         <!--
                        <select class="table--search--select" name="" id="">
                            <option value="">Filter</option>
                        </select>  -->
                    </div>

                    <!--///////////////////////////////////////////////////////////////////////////////////////////////////////////////////-->
                    <div class="relative">
                        <i class="table--search--input--icon fas fa-search "></i>
                        <input class="table--search--input" type="text" placeholder="Search invoice" v-model="searchInvoice" @keyup="search()">
                    </div>
                    <!--////////////////////////////////////////////////////////////////////////////////////////////////////////////////////-->

                </div>
                <!--Invoice List Table-->
                <div class="table--heading">
                    <p>ID</p>
                    <p>DATE</p>
                    <p>NUMBER</p>
                    <p>CUSTOMER</p>
                    <p>DUE_DATE</p>
                    <p>TOTAL</p>
                </div>

                <!-- item 1 -->
                <div class="table--items" v-for="item in invoices" :key="item.id" v-if="invoices.length">
                    <a href="#" @click="onShow(item.id)" style="color: blue;">
                        #{{ item.id }}
                    </a>
                    <p>{{ item.date }}</p>
                    <p>#{{ item.number }}</p>
                    <p v-if="item.customer">
                        {{ item.customer.firstname + " "+ item.customer.lastname }}
                    </p>
                    <p v-else></p>
                    <p>{{ item.due_date }}</p>
                    <p> $ {{ item.total }}</p>
                </div>
                <div class="table--items" v-else>
                    <p>invoice not found</p>
                </div>

            </div>

        </div>
        <!--  END of INVOICE LIST-------------------------------------------- -->
    </div>

</template>



<script setup>

import { onMounted, ref } from 'vue';

import { useRouter } from 'vue-router'


    const router = useRouter()

    let invoices = ref([])

    let searchInvoice =  ref([])

    // table data
    onMounted(async () => {
        getInvoices()
    })

    const getInvoices = async () => {
        let response = await axios.get("/api/get_all_invoice")
        console.log('response', response)
        invoices.value = response.data.invoices
    }

    // search bar
    const search = async () => {
        let response = await axios.get('/api/search_invoice?sasintha=' + searchInvoice.value)
        console.log('response', response.data.invoices) //in video =  response.data.invoices
        invoices.value = response.data.invoices
    }

    // newInvoice Button
    const newInvoice = async () => {
        let form = await axios.get("/api/create_invoice")
        console.log('form', form.data)
        router.push('/invoice/new')
    }

    const onShow = (id) => {
        router.push('/invoice/show/' + id)
    }


/*
export default {
  name: 'index',

  data () {
    return {
      result: {},
      invoice: {
        id: '',
        date: '',
        number: '',
        customer_id: '',
        due_date: '',
        total: '',
      }
    }
  },

  created () {
    this.EmployeeLoad()
  },

  methods: {
    EmployeeLoad () {
      var page = 'http://127.0.0.1:8000/api/get_all_invoice'
      axios.get(page)
        .then(
          ({data}) => {
            console.log(data)
            this.result = data
          }
        )
    },
  }
}
*/
</script>
