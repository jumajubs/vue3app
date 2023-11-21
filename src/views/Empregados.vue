<template>
  <div>
    <h1>Gerenciamento de Empregados</h1>

    
    <div>
      <h2>{{ empregados.length > 0 ? 'Lista de Empregados' : 'Resultados da Pesquisa' }}</h2>
      <table v-if="empregados.length > 0">
        <thead>
          <tr>
            <th>CTPS / Email</th>
            <th>Nome Completo</th>
            <th>Carga Horária</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="empregado in empregados" :key="empregado.id">
            <td>{{ empregado.ctps }} / {{ empregado.email }}</td>
            <td>{{ empregado.nomeCompleto }}</td>
            <td>{{ empregado.cargaHoraria }}</td>
          </tr>
        </tbody>
      </table>
      <p v-if="nenhumRegistroEncontrado">Nenhum registro foi encontrado para os critérios fornecidos</p>
    </div>

    
    <div>
      <h2>Cadastro de Empregado</h2>
      <form @submit.prevent="cadastrarEmpregado">
        <label for="ctps">CTPS:</label>
        <input v-model="ctps" type="text" required />

        <label for="email">Email:</label>
        <input v-model="email" type="email" required />

        <label for="nomeCompleto">Nome Completo:</label>
        <input v-model="nomeCompleto" type="text" required />

        <label for="cargaHoraria">Carga Horária:</label>
        <input v-model="cargaHoraria" type="text" required />

        <button type="submit">Cadastrar Empregado</button>
      </form>
    </div>

   
    <div>
      <h2>Consulta de Empregado</h2>
      <label for="ctpsConsulta">CTPS:</label>
      <input v-model="ctpsConsulta" type="text" required />

      <label for="emailConsulta">Email:</label>
      <input v-model="emailConsulta" type="email" required />

      <button @click="consultarEmpregado">Consultar</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'https://8080-jumajubs-springboot3app-fv29q7g0zbi.ws-us106.gitpod.io';

export default {
  data() {
    return {
      empregados: [],
      ctps: '',
      email: '',
      nomeCompleto: '',
      cargaHoraria: '',
      ctpsConsulta: '',
      emailConsulta: '',
    };
  },
  methods: {
    async cadastrarEmpregado() {
      try {
        const response = await axios.post(`${baseURL}/empregado`, {
          ctps: this.ctps,
          email: this.email,
          nomeCompleto: this.nomeCompleto,
          cargaHoraria: this.cargaHoraria,
        });

       
        this.empregados.push(response.data);

        this.ctps = '';
        this.email = '';
        this.nomeCompleto = '';
        this.cargaHoraria = '';
      } catch (error) {
        console.error('Erro ao cadastrar empregado:', error);
      }
    },
    async consultarEmpregado() {
      try {
        const response = await axios.get(`${baseURL}/empregado/${this.ctpsConsulta}/${this.emailConsulta}`);
        const empregado = response.data;

       
        this.empregados = empregado ? [empregado] : [];
        this.nenhumRegistroEncontrado = !empregado;
      } catch (error) {
        console.error('Erro ao consultar empregado:', error);
        this.nenhumRegistroEncontrado = true; 
      }
    },
    async carregarEmpregados() {
      try {
        const response = await axios.get(`${baseURL}/empregado`);
        this.empregados = response.data;
        this.nenhumRegistroEncontrado = this.empregados.length === 0;
      } catch (error) {
       
        this.nenhumRegistroEncontrado = true; 
      }
    },
  },
  created() {
 
    this.carregarEmpregados();
  },
};
</script>

<style>

</style>
