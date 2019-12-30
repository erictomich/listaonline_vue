<template>
  <div class="home">

    
    <!--<input type="text" v-model="buscaNome" placeholder="Buscar por nome">-->
    <br><br>
    <input type="text" v-model="buscaBairro" placeholder="Buscar por bairro">
    <br><br>
    <input type="text" v-model="buscaCategoria" placeholder="Buscar por Categoria">
<br><br>
    <input type="text" v-model="buscaKeyword" placeholder="Buscar por Nome ou Palavra-chave">

    <div v-for="entidade in list" :key="entidade.id">
      <p>{{ entidade.nome }} :: <strong>{{ entidade.bairro }}</strong> :: {{ entidade.telefone }} :: {{ entidade.categoria }}
      <br> 
      <span v-for="keyword in entidade.keywords" :key="keyword">{{ keyword }} :: </span>
      </p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  data() {
    return {
      configs: {
        orderBy: 'ordem',
        order: 'desc',
    },
      buscaNome: '',
      buscaBairro: '',
      buscaCategoria: '',
      buscaKeyword: '',
      entidades: [
        { ordem: 2, categoria: 'Automotivo', nome: 'Auto Moto Escola Renovar', bairro: "Santa Efigênia", telefone: '(31)3481 4422', keywords: ['Teste'] },
        { ordem: 5, categoria: 'Educação',  nome: 'Escola Infantil Oficina da Criança', bairro: "Esplanada", telefone: '(31)3461 4808', keywords: ['Teste'] },
        { ordem: 4, categoria: 'Educação',  nome: 'Escola Estadual Desembargador Rodrigues Campos', bairro: "Floresta", telefone: '(31)3331-1609', keywords: ['Auto aprendizagem', 'Ensino Fundamental', 'Ensino Básico'] },
        { ordem: 3, categoria: 'Automotivo', nome: 'Auto e Moto Escola Travessia', bairro: "Floresta", telefone: '(31)3024 1163 / 3222 8246', keywords: ['Teste'] },
        { ordem: 1, categoria: 'Música',  nome: 'Aulas de violão ou teclado', bairro: "Horto", telefone: '(31)3485.7873', keywords: ['Violão, Teclado, Música'] },
        { ordem: 6, categoria: 'Arte',  nome: 'ARTETERAPIA', bairro: "Esplanada", telefone: '(31)3485.7873', keywords: ['Terapia', 'pintura', 'Arte', 'Saúde', 'Estudo'] }
     ]
    }
  },
  computed: {
    filterNome() {
      return this.entidades.filter(entidade => {
        return entidade.nome.toLowerCase().includes(this.buscaKeyword.toLowerCase())
      });
    },
    filterBairro() {
      return this.entidades.filter(entidade => {
        return entidade.bairro.toLowerCase().includes(this.buscaBairro.toLowerCase())
      });
    },
    filterCategoria() {
      return this.entidades.filter(entidade => {
        return entidade.categoria.toLowerCase().includes(this.buscaCategoria.toLowerCase())
      });
    },
    filterKeywords() {
      const instanceVue = this;

      return this.entidades.filter(entidade => {

        var tamanho = entidade.keywords.length;
        var contem = false;

        for(var i=0;i<tamanho;i++) {
           if(entidade.keywords[i].toLowerCase().includes(this.buscaKeyword.toLowerCase())) {
             contem = true;
           }
        }

        return contem;

      })
    },
    filterTotal() {
      var palavraChave = _.concat(this.filterNome, this.filterKeywords)
      return _.intersection(palavraChave, this.filterBairro, this.filterCategoria);
    },
    list() {
      return _.orderBy(this.filterTotal, this.configs.orderBy, this.configs.order); 
    },
  },
  components: {
    HelloWorld
  }
}
</script>
