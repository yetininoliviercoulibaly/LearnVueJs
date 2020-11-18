<template>
      <v-row v-if="this.PageCount > 1">    
          <v-btn v-for="pageNumber in PageNumbers" 
             :key="pageNumber" 
             @click="SetCurrentPage(pageNumber)"
             v-bind:class="{'primary' : pageNumber == CurrentPageIndex}">
             {{pageNumber}}
          </v-btn> 
      </v-row>
</template>


<script>
export default {
    props :{
       PageCount : Number, 
       InitialPageIndex : Number
    },
    data(){
        return {
            CurrentPageIndex : 0,
        }
    },
    computed : {
      PageNumbers(){
          return [...Array(this.PageCount + 1).keys()].slice(1); 
      }
    },
    methods:{
      SetCurrentPage(pageindex){
          this.CurrentPageIndex = pageindex; 
      }
    },
    created(){
        this.CurrentPageIndex = this.InitialPageIndex; 
    }
}
</script>