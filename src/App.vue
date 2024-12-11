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

