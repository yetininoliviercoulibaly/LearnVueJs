<template>
      <div> 
           <div  v-if="TodosFiltered.length == 0"> 
               Vous n'avez aucune tâche  en attente
           </div>
           <div>
             <input style="  border: 2px solid black;border-radius: 4px;"  type="text" v-model="NewTaskLabel" />
             <a href="#" class="btn" v-on:click="AddNewTask" >
                 Ajouter une tâche. 
             </a>
           </div>
           <v-simple-table v-if="TodosFiltered.length > 0">
             <template v-slot:default>
               <thead>
                   <tr>
                       <th class="text-left">Id</th>
                       <th class="text-left">tâche</th>
                       <th class="text-left">Fait ?</th>
                       <th class="text-left">Action</th>
                    </tr>
               </thead>
               <tbody>
                    <tr v-for="todo in TodosFiltered" :key="todo.Id">
                       <td>
                           {{todo.Id}}
                       </td>
                       <td>
                           {{todo.Label}}
                       </td>
                       <td>
                           <input type="checkbox" v-model="todo.Done" />
                       </td>
                       <td>
                           <a href="#" @click="RemoveTask(todo.Id)" >Supprimer</a>
                       </td>
                    </tr>                   
               </tbody>
             </template>
           </v-simple-table>
           <div >
                <input type="checkbox" v-model="HideCompleted" /> 
                Cacher les tâches effectuer ? 
           </div>
           
      </div>
</template>


<script>
export default {
    props :{
        Tasks : Array,
        AddNewTaskFromParent : Function, 
        StorageName : String,
        PageSize : Number,
        CurrentPageIndex : Number, 
    },
    data() { return {
        HideCompleted : true, 
        NewTaskLabel : ""
    }},
    computed : {
        TodosFiltered() { return (this.HideCompleted ?  this.Tasks.filter(t => !t.Done) : this.Tasks).slice(this.CurrentPageIndex,this.CurrentPageIndex + this.PageSize + 1); } 
    },
    methods:{
         StoreData(){
            localStorage.setItem(this.StorageName, JSON.stringify(this.Tasks)); 
        },
        RemoveTask(id){
           this.Tasks = this.Tasks.filter(t => t.Id != id); 
           this.StoreData(); 
        },
        AddNewTask(){
            if(this.NewTaskLabel == "") return; 
            var maxId = Math.max(...this.Tasks.map(t=>t.Id)); 
            this.Tasks.push({
               Id : maxId + 1, 
               Label : this.NewTaskLabel,
               Done : false
            }); 
            this.NewTaskLabel = ""; 
            this.StoreData(); 
        },
        AddNewTask2(){
          if(this.NewTaskLabel == "") return; 
          this.AddNewTaskFromParent(this.NewTaskLabel);
          this.NewTaskLabel = "";  
        }
    }
}
</script>