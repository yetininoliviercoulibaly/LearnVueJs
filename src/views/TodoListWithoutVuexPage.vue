<template>
    <div>
      <todo-list StorageName="todos" 
      :Tasks="Tasks" 
      :AddNewTaskFromParent="AddNewTask"  
      :CurrentPageIndex="CurrentPageIndex" 
      :PageSize="PageSize"/>

      <paging-control 
      :PageCount="PageCount" 
      :InitialPageIndex="CurrentPageIndex" 
      :PageSize="PageSize"
      @CurrentPageChanged="HandleCurrentPageChanged" />
    </div>    
</template>


<script>
import TodoList from "@/components/TodoList.vue";
import PagingControl from "@/components/PagingControl.vue"; 

export default {
  name: 'MyTodoList',
  components: {
    TodoList,
    PagingControl
  }, 
  data() {
    return {
      Tasks : [], 
      PageSize : 2,
      CurrentPageIndex:1
    }
  },
  created(){
    let data = localStorage.getItem("todos");
    if(data != null){
      this.Tasks = JSON.parse(data); 
    } 
    else {
      this.Tasks = [
            { Id : 1 ,Label :"Accompagner alice à l'école", Done:false },
            { Id : 2 ,Label :"Accompagner Adrien à la crêche", Done:false},
            { Id : 3 ,Label :"Travailler", Done:false,  },
            { Id : 4 ,Label :"Dejeuner", Done:false },
            { Id : 5 ,Label :"Terminer sa journée", Done:false }
        ]; 
    }
    
  },
  computed :{
    PageCount(){
        return Math.ceil(this.Tasks.length/this.PageSize); 
    }
  },
  methods :{
    StoreData(){
       localStorage.setItem("todos", JSON.stringify(this.Tasks)); 
    },
    AddNewTask(newTaskLabel){
            if(newTaskLabel == "") return; 
            var maxId = Math.max(...this.Tasks.map(t=>t.Id)); 
            this.Tasks.push({
               Id : maxId + 1,  
               Label : newTaskLabel,
               Done : false
            });  
            this.StoreData(); 
        },
     HandleCurrentPageChanged(event, newIndexPage){
       this.CurrentPageIndex = newIndexPage; 
     }
  }
}
</script>