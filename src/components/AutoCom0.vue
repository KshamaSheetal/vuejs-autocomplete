<template>
<div class="autoComplete">
<!-- String search from JSON file  -->
  <h2>Normal query search</h2>

<input @keyup='tryAuto'  placeholder='Search for a query' />
<div class="sr" v-if="items.length>0">
<div id="results">
  <div v-for="item in items" v-bind:key="item.id">
    {{ item.Name }}
  </div>
</div>

</div>
</div>
</template>

<script>
import Autocomplete from '@trevoreyre/autocomplete-vue'
import '@trevoreyre/autocomplete-vue/dist/style.css'

import * as data from "../data/source.json"
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
      if (input.length < 1) { return [] }
      return this.queries.filter(query => {
        return query.Name.toLowerCase()
          .startsWith(input.toLowerCase())
      })
    },
      tryAuto:function(e){
        
        var searchStr= e.currentTarget.value;
        var that=this;
        var items=this.items=[];
        var regStr = new RegExp(`(\w*${searchStr}\w*)`, 'gi');
        if(searchStr.length>=1){
        that.items= that.queries.filter(query => {
                return query.Name.toLowerCase()
                  .includes(searchStr.toLowerCase())
              })
        }
      },
//Using vue autocomolete component
      getResultValue(result) {
      return result.Name
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