<script>
import BaseButton from '../components/BaseButton.vue';
import BaseInput from '../components/BaseInput.vue';
import BaseLabel from '../components/BaseLabel.vue';
import BaseNavLi from '../components/BaseNavLi.vue';
import Loader from '../components/Loader.vue';
import { getServicesData } from '../services/create-service'
import { deleteServiceByID } from '../services/delete-service'

export default {
  name: 'PanelAdministrador',
  components: { BaseButton, BaseLabel, BaseInput, BaseNavLi, Loader },
  data() {
    return {
      panelLoading: true,
      services: [],
    }
  },
  async mounted() {
    this.services = await getServicesData();
    this.panelLoading = false;
  },
  methods: {
    async deleteService(id) {
      const deleted = await deleteServiceByID(id);
      if (deleted) {
        this.services = this.services.filter(service => service.id !== id);
      }
    }
  }
}

</script>
<template>
  <div class="w-75 mx-auto">
    <h1>Panel Administrador</h1>

    <ul class="nav justify-content-center py-3">
      <BaseNavLi><router-link to="/panel" class="text-decoration-none text-black link-primary">Servicios</router-link>
      </BaseNavLi>
      <BaseNavLi><router-link to="/crear-servicio" class="text-decoration-none text-black link-primary">Crear un nuevo
          servicio</router-link>
      </BaseNavLi>
    </ul>

    <Loader v-if="panelLoading" />
    <template v-else>
      <div class="col-sm-12 col-md-8 col-lg-8 col-xl-8 py-4 bg-white  mx-auto">
        <h2>Servicios</h2>
        <table class="table">
          <thead>
            <tr>
              <th class="centrado">Servicio</th>
              <th class="centrado">Tiempo</th>
              <th class="centrado">Modalidad</th>
              <th class="centrado">Precio</th>
              <th class="centrado">Acciones</th>
            </tr>
          </thead>
          <template v-for="service in services" :key="service.id">
            <tbody>
              <tr>
                <td class="centrado">{{ service.name }}</td>
                <td class="centrado">{{ service.time }}</td>
                <td class="centrado">{{ service.modality }}</td>
                <td class="centrado">${{ service.price }}</td>
                <td class="centrado">
                  <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                    <button class="btn btn-warning me-md-2" type="button">Editar</button>
                    <button type="button" class="btn btn-danger" data-bs-toggle="modal" data-bs-target="#exampleModal">Eliminar</button>


                    <!-- Modal de bootstrap-->
                    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel"
                      aria-hidden="true">
                      <div class="modal-dialog">
                        <div class="modal-content">
                          <div class="modal-header">
                            <h1 class="modal-title fs-5" id="exampleModalLabel">
                              Eliminar: {{ service.name }}</h1>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body">
                            <p>¿Estás seguro que queres eliminar este servicio?</p>
                          </div>
                          <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                            <button type="button" class="btn btn-danger" @click="deleteService(service.id)" data-bs-dismiss="modal">Si, estoy seguro</button>
                          </div>
                        </div>
                      </div>
                    </div>

                  </div>
                </td>
              </tr>
            </tbody>

          </template>
        </table>
      </div>
    </template>
  </div>
</template>