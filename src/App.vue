<script setup>
  import {ref, onMounted} from 'vue';
const users = ref([
    { id: 1, name: "John Doe" },
    { id: 2, name: "Jane Doe" },
    { id: 3, name: "Peter Smith" },
    { id: 4, name: "Susan Jones" },
  ]);
  
  let selectedUser = ref(null);
  let queue = ref([]);

  // special functions
  function containsObject(obj, list){
    var i;
    for(i = 0;i<list.length;i++){
      if(list[i] === obj){
        return true;
      }
    }
    return false;
  }

  // special functions
  function removeObjectWithId(arr, id) {
  return arr.filter((obj) => obj.id !== id);
  }
  
  function selectUser(userId) {
    selectedUser.value = users.value.find((user) => user.id === userId);

    if(!containsObject(selectedUser.value, queue.value)){
      queue.value.push(selectedUser.value);
    }
  }
  
  function deselectUser(userId) {
    if(queue.value.length > 1){
      queue.value = removeObjectWithId(queue.value, userId);
      if(selectedUser.value.id === userId){
        selectedUser.value = queue.value[0]
      }
    }else{
      selectedUser.value = queue.value[0]
    }
  }
  
  function ensureAtLeastOneUserInQueue() {
    if (!selectedUser.value) {
      selectedUser.value = users.value[0];
      queue.value.push(selectedUser.value);
    }
  }
    
  onMounted(()=>{
    ensureAtLeastOneUserInQueue()
  })
  
</script>

<template>
  
  <ul v-for="user in users" :key="user.id">
    <div class="flex">
      <li @click="selectUser(user.id)">{{user.name}}</li>
      <button @click="deselectUser(user.id)">
        Deselect {{`${user.name}`}}
      </button>
    </div>
  </ul>

  <h5>
    The queue is :
  </h5>{{queue}}

  <h6>the selected user is: {{ selectedUser }}</h6>
</template>