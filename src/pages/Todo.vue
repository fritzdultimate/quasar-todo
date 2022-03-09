<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add Text"
        dense
        bg-color="white"
      >

        <template v-slot:append>
          <q-btn
            @click="addTask"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
     <q-list
      separator
      bordered
      class="bg-white">
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-blue-1': task.done }"
        v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { ref } from 'vue';
import { useQuasar } from 'quasar'

export default {
  setup() {
    const $q = useQuasar();
    const newTask = ref('');
    const tasks = ref([
      // {
      //   title: 'Get Bannanas',
      //   done: false
      // },
      // {
      //   title: 'Eat Bannanas',
      //   done: true
      // },
      // {
      //   title: 'Poo Bannanas',
      //   done: false
      // }
    ])

    function deleteTask(index) {
      $q.dialog({
        title: 'Confirm',
        message: 'Delete Task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        $q.notify('Task deleted')
      })

    }

    function addTask() {
      tasks.value.push({
        title: newTask.value,
        done: false
      })
      newTask.value = '';
    }

    return {
      tasks,
      newTask,
      addTask,
      deleteTask
    }
  }
}
</script>

<style lang="scss" scoped>
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }

  .no-tasks {
    opacity: 0.5;
  }
</style>
