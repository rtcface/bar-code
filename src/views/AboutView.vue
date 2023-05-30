<template>
  <div class="container text-center">
    <h1>Crea tus codigos de barras.</h1>

  <table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">Codigo de barras</th>
      <th scope="col">Producto</th>      
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td><vue-barcode value="PDESJKS8723984" :options="{ displayValue: true }"/></td>
      <td>Otto</td>      
    </tr>
    <tr>
      <th scope="row">2</th>
      <td><vue-barcode value="PDESJKS8723984" :options="{ displayValue: true }"/></td>
      <td>Thornton</td>      
    </tr>
    <tr>
      <th scope="row">3</th>
      <td><vue-barcode value="PDESJKS8723984" :options="{ displayValue: true }"/></td>
      <td>@twitter</td>
    </tr>
  </tbody>
</table>
  </div>
  <div class="about">    
    <input type="file"
      accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
       ref="excelSelector" 
      v-show="false" 
      @change="upLoadExcelInput">
    <button class="btn btn-primary"
         @click="upLoadExcel">
          Subir archivo de excel
          <i class="fa fa-upload"></i>
        </button>
    
  </div>
</template>

<script>
import { defineAsyncComponent } from 'vue'
import readXlsFile from 'read-excel-file'

export default {
    name: 'createBarcode',
    components:{    
    VueBarcode: defineAsyncComponent( () => import( /* webpackChunkName:VueBarcode  */ '@chenfengyuan/vue-barcode' ) )
  },
  data: () => ({
    excelItems:[],
    file:null
  }),
  methods:{
    async upLoadExcelInput( event ){  
      const file = event.target.files[0]
      if( !file ){
        this.excelItems=[]
        this.file=null
        return
      }
      this.file = file
      const result = await readXlsFile(this.file);
      console.log(result)

    },
    upLoadExcel(){
      this.$refs.excelSelector.click()
    }
  }
}

</script>
