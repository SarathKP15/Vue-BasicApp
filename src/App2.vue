<script setup>
import {ref, onMounted} from 'vue';

  const status = ref('active');
 const list = ref(['First Item', 'Second Item', 'Third Item']);
  const newItem=ref('');
  const toggleStatus = ()=>{
    if (status.value=='active'){
      status.value='inactive';
    }else{
      status.value='active';
    }
  }

  const addTask = ()=>{
    if(newItem.value.trim() !='' && !list.value.includes(newItem.value.trim())){
      list.value.push(newItem.value);
      newItem.value='';
    }
  }

  const removeItem=(itemIndex)=>{
    list.value.splice(itemIndex,1);
  }

  onMounted(async()=>{
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      list.value = data.map(x=>x.title);
    } catch (error) {
     console.log('Error fetching data') ;
      
    }
  })    ;
</script>

<template>
  <h1>Vue Jobs</h1>
  <span>User is: </span>
  <b class="bold-text">{{ status }}</b>
  <ul>
    <li v-for="(item,index) in list" :key="item">
      {{ item }}
      <button @click="removeItem(index)">X</button>
    </li>
  </ul>
  <br>
  <form @submit.prevent="addTask">
  <label for="Item">Item name:</label>
  <input type="text" id="itemId" v-model=newItem></input>
  <br>
  <button type="submit">Add Item</button>
</form>
<br>
  <button @click="toggleStatus">Change Status</button>
</template>

<style scoped>
.bold-text{
  font-weight: bold;
}
</style>