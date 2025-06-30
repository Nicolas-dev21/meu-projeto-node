<template>
  <div id="app">
    <h1>📋 Controle de Tarefas</h1>

    <!-- Campo para digitar nova tarefa -->
    <input
      v-model.trim="newTask"
      @keyup.enter="addTask"
      placeholder="Digite e pressione Enter para adicionar"
      aria-label="Campo para digitar nova tarefa"
    />

    <!-- Lista de tarefas -->
    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        class="task-item"
      >
        <!-- Texto da tarefa clicável -->
        <span
          class="clickable"
          :class="{ completed: task.completed }"
          @click="toggleTask(index)"
        >
          {{ task.text }}
        </span>

        <!-- Botão de excluir tarefa com ícone e acessibilidade -->
        <button @click="deleteTask(index)" aria-label="Excluir tarefa">
          🗑️
        </button>
      </li>
    </ul>

    <!-- Rodapé com nome e ano -->
    <footer>Desenvolvido por Nicolas Henrique - {{ new Date().getFullYear() }}</footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "", // Texto da nova tarefa
      tasks: []    // Lista de tarefas
    };
  },
  methods: {
    // Adiciona nova tarefa e aplica transição visual
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = "";
        this.saveTasks();
      }
    },
    // Alterna status concluído
    toggleTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveTasks();
    },
    // Exclui tarefa
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    // Salva tarefas no navegador
    saveTasks() {
      localStorage.setItem("tarefas", JSON.stringify(this.tasks));
    }
  },
  mounted() {
    // Carrega tarefas salvas no navegador
    const saved = localStorage.getItem("tarefas");
    if (saved) {
      this.tasks = JSON.parse(saved);
    }
  }
};
</script>

<style>
/* Estilo global do corpo da página */
body {
  background-color: #121212;
  color: #f0f0f0;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#app {
  text-align: center;
  max-width: 600px;
  margin: 40px auto;
}

/* Título */
h1 {
  font-size: 2.5em;
  color: #90caf9; /* Azul claro moderno */
  margin-bottom: 20px;
}

/* Campo de entrada */
input {
  padding: 10px;
  font-size: 16px;
  margin: 0 auto 15px auto;
  display: block;
  width: 80%;
  max-width: 400px;
  background-color: #1e1e1e;
  color: white;
  border: 1px solid #444;
  border-radius: 6px;
}
input::placeholder {
  color: #aaa;
}

/* Lista de tarefas */
ul {
  list-style-type: none;
  padding: 0;
}

/* Item da tarefa com transição */
.task-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  background-color: #1f1f1f;
  margin: 6px 0;
  border-radius: 6px;
  transition: all 0.3s ease-in-out;
}

/* Texto da tarefa */
li span {
  cursor: pointer;
  transition: color 0.2s ease;
}

/* Tarefa concluída */
li .completed {
  text-decoration: line-through;
  color: gray;
}

/* Hover da tarefa */
li span:hover {
  color: #00bfff;
  text-decoration: underline;
}

/* Botão excluir com ícone */
button {
  background-color: crimson;
  color: white;
  border: none;
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 6px;
  transition: background-color 0.3s;
  font-size: 16px;
}
button:hover {
  background-color: darkred;
}

/* Rodapé */
footer {
  margin-top: 40px;
  font-size: 0.9em;
  color: #888;
}
</style>

