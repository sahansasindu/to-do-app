<script setup>
import { ref, onMounted } from "vue";
import { db } from "../src/firebase.js";
import { collection, addDoc, getDocs, deleteDoc, doc } from "firebase/firestore";


const tasksCollection = collection(db, "tasks");

const tasks = ref([]);
const newTask = ref("");

// Load
const loadTasks = async () => {
  const snapshot = await getDocs(tasksCollection);
  tasks.value = snapshot.docs.map(doc => ({
    id: doc.id,
    ...doc.data()
  }));
};

// Add
const addTask = async () => {
  if (newTask.value.trim() !== "") {
    const docRef = await addDoc(tasksCollection, {
      text: newTask.value
    });
    tasks.value.push({ id: docRef.id, text: newTask.value });
    newTask.value = "";
  }
};

// Delete
const deleteTask = async (id) => {
  const confirmDelete = confirm("Are you sure you want to delete this task?");
  if (confirmDelete) {
    await deleteDoc(doc(db, "tasks", id));
    tasks.value = tasks.value.filter(task => task.id !== id);
  }
};

// Load
onMounted(() => {
  loadTasks();
});
</script>

<template>
  <div>
    <h1>TO DO LIST</h1>
    <form @submit.prevent="addTask">
      <label for="newTask">Add Task</label>
      <input type="text" id="newTask" v-model="newTask" />
      <button type="submit">ADD</button>
    </form>

    <h3>Tasks:</h3>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <span>{{ task.text }}</span>
        <button @click="deleteTask(task.id)">X</button>
      </li>
    </ul>
  </div>
</template>



<style scoped>
div {
  max-width: 400px;
  margin: 20px auto;
  padding: 15px;
  border-radius: 8px;
  background: #f5f1f1;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);

}
form {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button { padding: 8px 12px; border: none; border-radius: 6px; background-color: #4caf50; color: white; cursor: pointer; }

button:hover {
  background-color: #45a049;
}

h1{
  padding-bottom: 20px;
}

ul {
  list-style-type: none; padding: 0;
}

li {
  background: white;
  margin-bottom: 8px;
  padding: 8px 10px;
  border-radius: 6px;
  display: flex; justify-content: space-between;
  align-items: center;
  border: 1px solid #eee;
}

li span {
  font-size: 14px;
}

li button {
  background-color: #f44336;
  padding: 4px 8px;
}

li button:hover {
  background-color: #d32f2f;
}

</style>