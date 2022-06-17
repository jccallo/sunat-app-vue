<template>
   <div class="container mt-5">
      <!-- cuerpo -->
      <div class="row">
         <div class="col">
            <div class="card mb-4">
               <!-- card body -->
               <div class="card-body table-responsive">
                  <div class="row align-items-center mb-2">
                     <div class="col-sm-6 col-12">
                        <div class="h3">Cliente</div>
                     </div>
                     <div class="col-sm-6 col-12">
                        <div class="form-row form-inline justify-content-end">
                           <div class="col-auto my-1">
                              <a class="btn btn-primary" href="#"
                                 ><i class="bi bi-plus-circle"></i>Guardar</a
                              >
                           </div>
                        </div>
                     </div>
                  </div>

                  <form @submit.prevent="storeRol">
                     <div class="form-row">
                        <div class="col">
                           <div class="form-group">
                              <label>Numero de Documento:</label>
                              <select
                                 class="form-control"
                                 v-model="client.document_type"
                                 required
                              >
                                 <option>DNI</option>
                                 <option>RUC</option>
                                 <option>CE</option>
                                 <option>Pasaporte</option>
                              </select>
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <label>Numero de Documento:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.document_number"
                                 required
                              />
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <div class="form-group">
                                 <label>&nbsp;</label><br />
                                 <button
                                    type="button"
                                    class="btn btn-primary"
                                    @click.stop="search"
                                 >
                                    Buscar en Sunat
                                 </button>
                              </div>
                           </div>
                        </div>
                        <div class="col"></div>
                     </div>

                     <div class="form-row">
                        <div class="col">
                           <div class="form-group">
                              <label>Nombres y Apellidos:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.fullname"
                                 required
                              />
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <label>Mostrar Nombre Como (Alias):</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.username"
                                 required
                              />
                           </div>
                        </div>
                     </div>

                     <div class="form-row">
                        <div class="col">
                           <div class="form-group">
                              <label>Direccion:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.address"
                                 required
                              />
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <label>Ubigeo (Distrito):</label>
                              <select
                                 class="form-control"
                                 v-model="client.district_id"
                              >
                                 <option
                                    v-for="(district, key) in districts"
                                    :key="key"
                                    :value="key"
                                 >
                                    {{ district }}
                                 </option>
                              </select>
                           </div>
                        </div>
                     </div>

                     <div class="form-row">
                        <div class="col-6">
                           <div class="form-group">
                              <label>Email:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.email"
                              />
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <label>Telefono Movil:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.cellphone"
                              />
                           </div>
                        </div>
                        <div class="col">
                           <div class="form-group">
                              <label>Telefono Fijo:</label>
                              <input
                                 type="text"
                                 class="form-control"
                                 v-model="client.phone"
                              />
                           </div>
                        </div>
                     </div>
                  </form>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
import axios from "axios";

export default {
   name: "EditClient",
   data() {
      return {
         client: {
            document_type: "DNI",
            document_number: "",
            fullname: "",
            username: "",
            address: "",
            district_id: "",
            email: "",
            cellphone: "",
            phone: "",
         },
         districts: {}
      };
   },
   mounted(){
        this.loadCombos()
    }, 
   methods: {
      loadCombos() {
         axios
            .get(`http://localhost:8000/api/clients/combos`)
            .then((data) => {
               this.districts = data.data.districts;
               console.log(data.data.districts);
            })
            .catch((error) => {
               console.log(error);
            });
      },
      search() {
         let client = new FormData();
         client.append('document_type', this.client.document_type);
         client.append('document_number', this.client.document_number);

         axios
            .post(`http://localhost:8000/api/clients/search`, client)
            .then((data) => {
               if (!data.data.client) {
                  throw data;
               }
               else {
                  this.client = data.data.client;
                  console.log(data);
               }
            })
            .catch((error) => {
               this.client = {
                  document_type: this.client.document_type,
                  document_number: this.client.document_number,
                  fullname: "",
                  username: "",
                  address: "",
                  district_id: "",
                  email: "",
                  cellphone: "",
                  phone: "",
               }
               alert("No se encontro al cliente con ese documento."),
               console.log(error);
            });
      },
   },
};
</script>