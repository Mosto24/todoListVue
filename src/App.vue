<template>
  <div class="main">
    <v-btn variant="elevated" color="primary" class="btn" @click="formActive=!formActive">{{ formActive? 'Отменить создание': 'Создать задачу'}}</v-btn>
  </div>
  <div class="form" v-if="formActive">
    <v-form>
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="firstname"
            label="Название задачи"
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="lastname"
            label="Описание задачи"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-btn @click="addTask">Создать</v-btn>
    </v-container>
  </v-form>
  </div>
  <div class="tasks">
    <v-row>
      <v-col
      cols="12"
      md="12"
      v-for="task in taskList"
      >
        
    <v-card
      class="mx-auto"
      width="400"
      style="margin-top: 15px;"
    >
      <template v-slot:title>
        {{ task.title }}
      </template>

      <v-card-text>
        {{ task.body }}
      </v-card-text>
      <v-btn style="margin-bottom: 10px; margin-left: 10px;" @click="deleteTask(task.id)">Выполненно</v-btn>
    </v-card>
      </v-col>
    </v-row>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        formActive: false,
        firstname: '',
        lastname: '',
        taskList: [
        ],
      }
    },
    methods: {
      addTask() {
        this.taskList.push({
          id: Date.now(), title: this.firstname, body: this.lastname
        });
        this.firstname = '';
        this.lastname = '';
        this.formActive = false;
        localStorage.removeItem("list");
        localStorage.setItem('list', JSON.stringify(this.taskList));
      },
      deleteTask(id) {
        this.taskList = this.taskList.filter((item) => item.id != id);
        localStorage.removeItem("list");
        localStorage.setItem('list', JSON.stringify(this.taskList))
      }
    },
    mounted() {
      let local = localStorage.getItem('list');
      if(local == null) {

      } else {
        this.taskList = JSON.parse(local);
      }
    }
  }
</script>

<style>
  .main {
    display: flex;
    justify-content: space-around;
  }
  .btn {
    margin-top: 10px;
  }
  .form {
    background-color: aliceblue;
    width: 50%;
    margin: 0 auto;
    margin-top: 10px;
    border: 5px solid rgb(59, 86, 110);
  }
  .tasks {
    display: flex;
    justify-content: space-between;
  }
</style>