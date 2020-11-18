<template>
    <div>
      <task-list 
      @OnRemoveTask="HandleRemoveTask" 
      @OnAddNewTask= "HandleAddNewTask"
      @OnHideCompletedChanged = "HandleHideCompletedChanged"
      StorageName="todos" 
      :Tasks="FilteredTasks"
      />
    </div>    
</template>


<script>
import TaskList from "@/components/TaskList.vue";

export default {
  name: 'MytaskList',
  components: {
    TaskList
  }, 
  data() {
    return {
      Tasks : [],
      HideCompleted : false
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
  computed : {
    FilteredTasks(){
      return this.HideCompleted ?  this.Tasks.filter(t => !t.Done) : this.Tasks
    }
  },
  methods :{
    StoreData(){
       localStorage.setItem("todos", JSON.stringify(this.Tasks)); 
    },
     HandleAddNewTask(newTaskLabel){
          if(newTaskLabel == "") return ;
           var maxId = Math.max(...this.Tasks.map(t=>t.Id)); 
            this.Tasks.push({
               Id : maxId + 1,  
               Label : newTaskLabel,
               Done : false
            });  
            this.StoreData(); 
     },
     HandleRemoveTask(taskId){
         this.Tasks = this.Tasks.filter(o => o.Id != taskId); 
         this.StoreData(); 
     }, 
     HandleHideCompletedChanged(value){
       this.HideCompleted = value; 
     }
  }
}
</script>