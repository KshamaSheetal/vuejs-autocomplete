<template>
<div class="autoComplete">
  <h2>Text search from Wikipedia</h2>

  <!-- Input second -->
<input @keyup='tryAuto'  placeholder='Search for a query' />
<div class="sr" v-if="items.length>0">
<div id="results">
  <div v-for="item in items" v-bind:key="item.id" @click="handleSubmit">
    {{ item.title }}
  </div>
</div>

</div>
</div>
</template>

<script>
import Autocomplete from '@trevoreyre/autocomplete-vue'
import '@trevoreyre/autocomplete-vue/dist/style.css'


import * as data from "../data/source.json"
const wikiUrl = 'https://en.wikipedia.org'
const params = 'action=query&list=search&format=json&origin=*'

export default {
  name: 'autoComplete',
    components: {
    Autocomplete
  },

  data: function () {
    return {

      items: [],
      fields: { value: 'Name' },
            filterType: 'Contains',
            queriesData: data['queries'],
            queries: data['queries'],
            
            width: '150px',
            filterData: ['Contains', 'StartsWith', 'EndsWith']
    }
  },
 
  methods:{
    
    
    // Using regular expession
    search(input) {
           const url = `${wikiUrl}/w/api.php?${
        params
      }&srsearch=${encodeURI(input)}`

      return 
      
    },
     handleSubmit(result) {
      window.open(`${wikiUrl}/wiki/${
        encodeURI(result.title)
      }`)
    },
      tryAuto:function(e){
        
        var searchStr= e.currentTarget.value;
            const url = `${wikiUrl}/w/api.php?${
        params
      }&srsearch=${encodeURI(searchStr)}`
        var that=this;
        var items=this.items=[];
        var regStr = new RegExp(`(\w*${searchStr}\w*)`, 'gi');
        if(searchStr.length>=1){
        
       
         return new Promise(resolve => {
        if (searchStr.length < 3) {
          return resolve([])
        }

        fetch(url)
          .then(response => response.json())
          .then(data => {
            that.items=data.query.search;
            resolve(data.query.search)
          })
      });
        }
      },
      
//Using vue autocomolete component
      getResultValue(result) {
      return result.title
    },
  }
}
</script>


<style scoped>
.autoComplete{
width:400px;
margin: 0 auto;

}
.autoComplete input{
    height:40px;
    width: 100%;
}
.sr{
  border-left: 1px solid #ccc;
    /* border-bottom: 1px solid #ccc; */
    border-right: 1px solid #ccc;
    width: 100%;
    padding: 8px 2px 8px 1px;
    text-align: left;
    box-shadow: 0 4px 5px 0px #bababa;
}
.sr>div>div{
  padding:8px 4px;
  margin-bottom: 8px;

}
.sr>div#results>div:hover{
  cursor: pointer !important;
  background-color: #a9d1ec !important; 
}
</style>