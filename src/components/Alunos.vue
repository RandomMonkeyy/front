<template>
  <div>
    <router-link to="/alunos/add" style="float: left; margin-right: 10px;">
      <button class="btn btn-primary">Adicionar</button>
    </router-link>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Codigo</th>
          <th>Nome</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="aluno in alunos" :key="aluno.codigoaluno">
          <td>{{ aluno.codigoaluno }}</td>
          <td>{{ aluno.nome }}</td>
          <td>
            <button class="btn btn-success" @click="visualizarAluno(aluno.codigoaluno)">Ver</button>
            <button  class="btn btn-info" @click="editarAluno(aluno.codigoaluno)">Editar</button>
            <button  class="btn btn-danger" @click="deleteAluno(aluno.codigoaluno)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      alunos: []
    };
  },
  created() {
    axios.get('http://localhost:3000/alunos').then(response => {
      console.log(response.data);
      this.alunos = response.data;
    });
  },
  methods: {   
    deleteAluno(codigocurso) { 
      axios.get('http://localhost:3000/matriculasPorcurso/'+codigoCurso).then(response => {
        if ( response.data.length == 0 ) {
          axios.delete('http://localhost:3000/cursos/'+codigocurso).then(() => {
            // Atualiza a lista de cursos
            this.cursos = this.cursos.filter(curso => curso.codigocurso !== codigocurso);
          });  
        } else {
          alert('Não é possível excluir pois o curso está matrículado em algum curso');
        }
      });                      
    },
    visualizarAluno(codigoaluno) {
      this.$router.push({ name: 'alunosvisualizar', params: { codigoaluno } });
    }
  }
  
};

</script>

<style>
.btn-success {
font-size: 10px;
}
.btn-info {
font-size: 10px;
}
.btn-danger {
font-size: 10px;
}
</style>