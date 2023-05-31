<template>
  <div class="container text-center">
    <h1>Crea tus codigos de barras.</h1>
    <input type="file"
      accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
       ref="excelSelector" 
      v-show="false" 
      @change="upLoadExcelInput">
    <button class="btn btn-primary mx-4"
         @click="upLoadExcel">
          Subir archivo de excel
          <i class="fa fa-upload"></i>
        </button>
  
    <button class="btn btn-secondary"
         @click="imprimirCode">
          Descargar archivo
          <i class="fa fa-download"></i>
        </button>
    
    <table class="table" v-if="excelItems" id="element-to-pdf">
      <thead>
        <tr>        
          <th scope="col">Codigo de barras</th>
          <th scope="col">Producto</th>      
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in excelItems"
          :key="product[0]"
          :product="product"
        >         
          <td><vue-barcode :value="product[0]" :options="{ displayValue: true }"/></td>
          <td>{{product[1]}}</td>      
        </tr>   
      </tbody>
    </table>
  </div>

</template>

<script>
import { defineAsyncComponent } from 'vue'
import readXlsFile from 'read-excel-file'
import $ from 'jquery'


export default {
    name: 'createBarcode',
    components:{    
    VueBarcode: defineAsyncComponent( () => import( /* webpackChunkName:VueBarcode  */ '@chenfengyuan/vue-barcode' ) )
  },
  data: () => ({
    excelItems:null,
    file:null
  }),
  methods:{
    async upLoadExcelInput( event ){  
      const file = event.target.files[0]
      if( !file ){
        this.excelItems=null
        this.file=null
        return
      }
      this.file = file
      this.excelItems = await readXlsFile(this.file);

      //console.log()

    },
    upLoadExcel(){
      this.$refs.excelSelector.click()
    },
    imprimirCode()
    {
      let element = document.getElementById('element-to-pdf');
      $('<iframe>', {name: 'myiframe',class: 'printFrame'})
        .appendTo('body')
        .contents().find('body')
         .append(element)
        window.frames['myiframe'].focus();
        window.frames['myiframe'].print();

        setTimeout(() => { $(".printFrame").remove(); }, 1000);
      //html2pdf(element)
    }
  }
}

</script>
