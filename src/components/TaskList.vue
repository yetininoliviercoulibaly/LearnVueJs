<template>
      <div> 
           <div  v-if="Tasks.length == 0"> 
               Vous n'avez aucune tâche  en attente
           </div>
           <div>
             <input style="  border: 2px solid black;border-radius: 4px;"  type="text" v-model="NewTaskLabel" />
             <a href="#" class="btn" v-on:click="AddNewTask" >
                 Ajouter une tâche. 
             </a>
           </div>
           <v-simple-table v-if="Tasks.length > 0">
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
                    <tr v-for="todo in Tasks" :key="todo.Id">
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
                <input type="checkbox"  @click="HideCompletedChanged" /> 
                Cacher les tâches effectuer ? 
           </div>
           
      </div>
</template>


<script>
export default {
    props :{
        Tasks : Array
    },
    data() { return {
        NewTaskLabel : ""
    }},
    methods:{
        RemoveTask(id){
            console.log("id from RemoveTask : " + id);
            this.$emit("OnRemoveTask",id); 
        },
        AddNewTask(){
           this.$emit("OnAddNewTask", this.NewTaskLabel);
           this.NewTaskLabel = "";  
        },
        HideCompletedChanged(event){
          this.$emit("OnHideCompletedChanged",event.target.checked); 
        }   
     }
}
</script>