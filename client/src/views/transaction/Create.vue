<template>
   <div class="container my-5">
      <div class="row justify-content-center">
         <div class="col-md-8 col-12">
            <router-link
               class="btn btn-sm btn-primary rounded shadow mb-3"
               :to="{ name: 'transaction.index' }"
               >Back</router-link
            >

            <div class="card shadow rounded">
               <div class="card-header text-uppercase fw-bold">
                  Add new transaction
               </div>
               <div class="card-body">
                  <form @submit.prevent="store()">
                     <div class="mb-3">
                        <label for="title" class="form-label">Title</label>
                        <input
                           type="text"
                           class="form-control"
                           id="title"
                           placeholder="input your transaction title"
                           v-model="transaction.title"
                        />
                        <div v-if="validation.title" class="text-danger">
                            {{ validation.title[0] }}
                        </div>
                     </div>

                     <div class="mb-3">
                        <label for="amount" class="form-label">Amount</label>
                        <input
                           type="number"
                           class="form-control"
                           id="amount"
                           placeholder="input your transaction amount"
                           v-model="transaction.amount"
                        />
                        <div v-if="validation.amount" class="text-danger">
                            {{ validation.amount[0] }}
                        </div>
                     </div>

                     <div class="mb-3">
                        <label for="time" class="form-label">Time</label>
                        <input
                           type="text"
                           class="form-control"
                           id="time"
                           placeholder="yyyy/mm/dd hh:mm:ss"
                           v-model="transaction.time"
                        />
                        <div v-if="validation.time" class="text-danger">
                            {{ validation.time[0] }}
                        </div>
                     </div>

                     <div class="mb-3">
                        <label for="type" class="form-label">Type</label>
                        <select
                           class="form-select form-select-sm"
                           aria-label=".form-select-sm example"
                           v-model="transaction.type"
                        >
                           <option selected disabled>Select type...</option>
                           <option value="expense">Expense</option>
                           <option value="revenue">Revenue</option>
                        </select>
                        <div v-if="validation.type" class="text-danger">
                            {{ validation.type[0] }}
                        </div>
                     </div>

                     <button
                        class="btn btn-outline-primary btn-sm float-end"
                     >
                        Submit
                     </button>
                  </form>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
    setup(){
        // data bindings
        const transaction = reactive({
            title: '',
            amount: '',
            time: '',
            type: ''
        });

        // validation
        const validation = ref([]);

        // useRouter
        const router = useRouter();

        function store(){
            axios.post('http://localhost:8000/api/transactions', transaction)
            .then(() => {
                router.push({
                    name: 'transaction.index'
                })
            }).catch((err) => {
                validation.value = err.response.data
            });
        }

        return {
            transaction,
            validation,
            router,
            store
        };
    }
}

</script>