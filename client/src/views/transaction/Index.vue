<template>
   <div class="container my-5">
      <div class="row justify-content-center">
         <div class="col-md-8 col-12">
            <router-link
               class="btn btn-sm btn-primary rounded shadow mb-3"
               :to="{ name: 'transaction.create' }"
               >Add</router-link
            >

            <div class="card shadow rounded">
               <div class="card-header">Transactions List</div>
               <div class="card-body">
                  <table class="table">
                     <thead>
                        <tr>
                           <th>Title</th>
                           <th>Amount</th>
                           <th>Type</th>
                           <th>Actions</th>
                        </tr>
                     </thead>
                     <tbody>
                        <tr
                           v-for="(transaction, index) in transactions.data"
                           key="index"
                        >
                           <td>{{ transaction.title }}</td>
                           <td>{{ transaction.amount }}</td>
                           <td>{{ transaction.type }}</td>
                           <td>
                              <div class="btn-group">
                                 <router-link
                                    class="btn btn-sm btn-outline-info"
                                    :to="{
                                       name: 'transaction.edit',
                                       params: { id: transaction.id },
                                    }"
                                 >
                                    Edit
                                 </router-link>
                                 <button
                                    class="btn btn-sm btn-outline-danger"
                                    @click.prevent="
                                       destroy(transaction.id, index)
                                    "
                                 >
                                    Delete
                                 </button>
                              </div>
                           </td>
                        </tr>
                     </tbody>
                  </table>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
import axios from "axios";
import { onMounted, ref } from "vue";
export default {
   setup() {
      // reactive state
      let transactions = ref([]);

      onMounted(() => {
         // get data from API Endpoint with axios request
         axios
            .get("http://localhost:8000/api/transactions")
            .then((result) => {
               transactions.value = result.data;
            })
            .catch((error) => {
               console.log(error.response);
            });
      });

      function destroy(id, index) {
         axios
            .delete(`http://localhost:8000/api/transactions/${id}`)
            .then(() => {
               transactions.value.data.splice(index, 1);
            })
            .catch((err) => {
               validation.value = err.response.data;
            });
      }

      return {
         transactions,
         destroy
      };
   }
};
</script>