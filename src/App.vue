<script setup>
import { ref } from 'vue';
//variável com tarefas
const tarefas = ref([
  { texto: "Correr", concluida: false},
  { texto: "Fazer compras", concluida: false },
  { texto: "Estudar", concluida: false },
  { texto: "Passear com o cachorro", concluida: false },
  { texto: "Ler livros", concluida: false },
  { texto: "Treinar", concluida: false }
])

// Alterei a estrutura da lista para que cada tarefa seja representada como um objeto com duas propriedades: o TEXTO e um BOOLEANO que indica se ela foi concluída

// Função para remover
function removerTarefa(index) {
  tarefas.value.splice(index, 1) 
  //usamos o .value para podemos acessar o valor de um dado reativo. O splice remove, substitui, podemos usar por ser um array.
}


// tarefa é cada item separado e index a sua posiçao
//a gente usa o key pra garantir que o item esteja na posiçao correta e quando formos deletar ele delete o certo. 


// Variavel para nova tarefa
const novaTarefa = ref("");

// Função de adicionar:
function adicionarTarefa(){
  if(novaTarefa.value.trim() !== ""){
    removerConcluidas(); // Remove as tarefas concluídas antes de adicionar nova
    tarefas.value.push({ texto: novaTarefa.value.trim(), concluida: false });
    novaTarefa.value = "" //Limpa o input
  }
}

//trim remove os espaços do inicio e do final


// Função para alternar o status de conclusão da tarefa
function marcarConcluida(index) {
  tarefas.value[index].concluida = !tarefas.value[index].concluida;} 

  //essa função marca a tarefa como concluída, tornando ela true



// Função para remover todas as tarefas concluídas ao adicionar nova tarefa
function removerConcluidas() {
  tarefas.value = tarefas.value.filter(tarefa => !tarefa.concluida);
}

//Essa função filtra a lista de tarefas, mantendo apenas as tarefas que não estão concluídas, removendo todas as tarefas concluídas.

</script>


<template>
  <h1>📝Lista de tarefas</h1>
   <section>
    <div>
    <input type="text"
    placeholder="Digite sua nova tarefa"
    v-model="novaTarefa" @keyup.enter="adicionarTarefa"></div>
    <button class="add" @click="adicionarTarefa">Adicionar</button>   
    <!--@keyup.enter: Este evento é ativado quando a tecla Enter é pressionada enquanto o campo de input está em foco.-->

  </section>
  <section class="lista">
  <!-- Verifica se todas as tarefas estão concluídas e exibe a mensagem -->
  <div v-if="tarefas.length > 0 && tarefas.every(tarefa => tarefa.concluida)">
      <p>Todas as tarefas concluídas! Você é a mais mais! 💅</p>
    </div>

    <!-- Mensagem quando não há tarefas -->
    <div v-else-if="tarefas.length === 0">
      <p>Não há tarefas pendentes</p>
    </div>

    <!-- /////////
    
    v-if="tarefas.length > 0 && tarefas.every(tarefa => tarefa.concluida)": Esse v-if verifica se existe pelo menos uma tarefa na lista e se todas as tarefas estão concluídas. Caso isso seja verdade, ele exibe a mensagem "Todas as tarefas concluídas!".

    v-else-if="tarefas.length === 0": Esse v-else-if só será executado se a lista de tarefas estiver vazia, exibindo a mensagem "Não há tarefas pendentes".
    
    ///////// -->
  
  
<!-- Quando usamos variáveis reativas dentro do TEMPLATE a gente NAO precisa usar o .value-->

  <ul v-else>
    <!-- v-for é Loop, estamos usando para renderizar cada elemento-->
    <li v-for="(tarefa, index) in tarefas" :key="index">
      <!-- Aplica a classe 'sublinhada' se a tarefa estiver concluída -->
      <span :class="{ sublinhada: tarefa.concluida }">{{ tarefa.texto }}</span>
      <button class="check" @click="marcarConcluida(index)">✔</button>
      <button class="remover" @click="removerTarefa(index)">❌</button></li>  <!--index é para identificar a posição do objeto no array-->
  </ul>
</section>
</template>

<style lang="scss">
:root {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.5;
  font-weight: 400;

  color-scheme: light dark;
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

a {
  font-weight: 500;
  color: #646cff;
  text-decoration: inherit;

  &:hover {
    color: #535bf2;
  }
}

body {
  margin: 0;
  display: flex;
  place-items: center;
  min-width: 320px;
  min-height: 100vh;
}

h1 {
  font-size: 3.2em;
  line-height: 1.1;
}

input {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  flex-grow: 1;
  font-size: 1rem;
  line-height: 1rem;
}

button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.2em 1em;
  margin-left: 0.5em;
  margin-bottom: 0.7em;
  font-size: 0.8em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;

  &.add {
    margin-top: 1em;
  }

  &.check:hover {
    border-color: #64ffaa;
  }

  &.remover:hover {
    border-color: #ff6464;
  }

  &:focus,
  &:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
  }
}

.card {
  padding: 2em;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

@media (prefers-color-scheme: light) {
  :root {
    color: #213547;
    background-color: #ffffff;
  }
  a:hover {
    color: #747bff;
  }
  button {
    background-color: #f9f9f9;
  }
}

.sublinhada {
  text-decoration: line-through;
  color: gray;
}

ul {
  list-style: none;
  padding-right: 1em;
  padding-left: 1em;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 8px;
}

span {
  flex: 1;
  text-align: left;
  margin-right: 16px;
}

.lista {
  border: solid #535bf2;
  border-radius: 18px;
}
</style>
