<template>
  <section class="sectionPainelControle">
    <div id="painelControle">
      <CardFormCadastro v-if="exibirCardFormCadastro" :acao="acaoCrud" :usuarioId="usuarioId"
        @enviarForm="atualizarLista" @ocultarForm="ocultarForm" />
      <CardConfirm style="display:none" />
      <div id="cabecalhoPainel">
        <div>
          <img class="logo_painel" src="./../../public/img/icons/android-chrome-maskable-192x192.png" alt="">
        </div>
        <div>
          <p>Painel Controle</p>
        </div>
      </div>
      <div id="centralPainel">
        <div id="crudUsuario">
          <div id="abasCrud">
            <div id="abaUsuario">
              <img class="iconeAvatar" src="../assets/avatar_icon.png" alt="icone_avatar">
              <p>Usuários</p>
            </div>
          </div>
          <div id="cabecalhoCrud">
            <div>
              <input type="text" name="" placeholder="Pesquisar..." id="" style="visibility: hidden;">
            </div>
            <div class="areaBtns">
              <BtnDefault :type="'button'" :value="'Edit'" v-show="!(usuarioId == null)" v-on:click="editarSelecionado"
                :preenchido="false" />
              <BtnDefault :type="'button'" :value="'Detalhes'" v-show="!(usuarioId == null)"
                v-on:click="exibirDadosUsuario" :preenchido="true" />
              <BtnDefault :type="'button'" :value="'Criar'" v-show="usuarioId == null" v-on:click="criarUsuario"
                :preenchido="true" />
            </div>
          </div>
          <div id="tabelaCrud">
            <div id="tituloTabela">
              <input type="checkbox" name="" id="" style="visibility: hidden;">
              <p>ID</p>
              <p>Nome</p>
              <p>Status</p>
              <p>Tipo</p>
              <p>Criação</p>
            </div>
            <div id="linhasTabela">
              <LinhaCrudUsuario v-for="linha in linhas" :key="linha.usuarioId" @salvarId="pegarIdRadio"
                :linhaData="linha" />
            </div>
          </div>
        </div>
        <div id="barraEdicao" style="display: none;">
          <div>
            x
            <p>3</p>
            <p>Itens selcionados</p>
          </div>
          <div>
            <input type="button" value="Edit" v-on:click="editarSelecionado">
            <input type="button" value="Delete" v-on:click="inativarSelecionado">
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import LinhaCrudUsuario from '@/components/LinhaCrudUsuario.vue'
import CardFormCadastro from '@/components/CardFormCadastro.vue'
import CardConfirm from '@/components/CardConfirm.vue'
import BtnDefault from "@/components/BtnDefault.vue";
export default {
  data() {
    return {
      linhas: [],
      exibirCardFormCadastro: false,
      acaoCrud: '',
      usuarioId: null,
      linhaSelecionada: ''
    };
  },
  methods: {
    criarUsuario() {
      this.acaoCrud = 'criar'
      this.exibirCardFormCadastro = true
    },
    exibirDadosUsuario() {
      this.acaoCrud = 'exibir'
      this.exibirCardFormCadastro = true
    },
    editarSelecionado() {
      this.acaoCrud = 'editar'
      this.exibirCardFormCadastro = true
    },
    inativarSelecionado() {
      axios
        .put(`http://localhost:8000/usuarios/inativar/${this.usuarioId}`)
        .then((response) => {
          console.log(response);
        })
        .catch((error) => (this.msg = error.response));
    },
    ativarSelecionado() {
      axios
        .put(`http://localhost:8000/usuarios/ativar/${this.usuarioId}`)
        .then((response) => {
          console.log(response);
        })
        .catch((error) => (this.msg = error.response));
    },
    atualizarLista() {
      axios
        .get("http://localhost:8000/usuarios")
        .then((response) => {
          console.log(response.data)
          this.linhas = response.data
          // this.ocultarCard()
        })
        .catch((error) => (this.msg = error.response));
    },
    ocultarForm(data) {
      this.exibirCardFormCadastro = data
    },
    pegarIdRadio(id) {
      this.usuarioId = id
    }
  },
  created() {
    axios
      .get("http://localhost:8000/usuarios")
      .then((response) => {
        console.log(response.data)
        this.linhas = response.data
      })
      .catch((error) => (this.msg = error.response));
  },
  components: {
    LinhaCrudUsuario,
    CardFormCadastro,
    CardConfirm,
    BtnDefault
  }
};
</script>

<style scoped>
.sectionPainelControle {
  width: 100vw;
  display: flex;
  justify-content: center;
}

input[type='button'] {
  padding: 2% 10%;
  border-radius: 3px;
  cursor: pointer;
}

.btnClaro {
  background-color: #ffffff;
  color: #ce2020;
  border: 1px solid #ce2020;
}

.btnEscuro {
  background-color: #ce2020;
  color: #ffffff;
  border: 1px solid transparent;
}

#painelControle {
  width: 80vw;
  min-height: 70vh;
  border-radius: 10px;
  margin-top: 10vh;
  padding: 1vw;
  background-color: rgb(243, 243, 247);
  box-shadow: 5px 5px 3px #c7c7c7,
    -5px -5px 3px #f9f9f9;
}

.logo_painel {
  aspect-ratio: 1/1;
  width: 2rem;
  height: 2rem;
  border-radius: 100%;
}

#centralPainel {
  min-height: 50vh;
  background-color: rgb(242, 242, 242);
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 20px;
  box-shadow: inset 5px 5px 3px #c7c7c7,
    inset -5px -5px 3px #f9f9f9;
}

#cabecalhoPainel {
  display: flex;
  align-items: center;
  margin-inline: 1%;
  gap: 1%;
}

#crudUsuario {
  width: 98%;
  height: 98%;
  border-radius: 20px;
}

#abasCrud {
  display: flex;
  background-color: white;
  width: fit-content;
  padding-inline: 2vw;
  border-top-left-radius: 10px;
  border-top-right-radius: 25px;
  border-bottom-right-radius: -30px;
  margin-top: 3vh;
}

#abasCrud div {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 5%;
  padding: 10% 0%;
}

#abasCrud div p {
  margin: 0;
}

.iconeAvatar {
  aspect-ratio: 1/1;
  width: 1rem;
  height: 1rem;
}

#cabecalhoCrud {
  display: flex;
  justify-content: space-between;
  background-color: white;
  width: 100%;
  padding: 2vh 0px;
  border-top-right-radius: 20px;
}

#cabecalhoCrud div {
  margin-inline: 2%;
}

.areaBtns {
  display: flex;
  flex-wrap: wrap;
  min-width: 30%;
  justify-content: flex-end;
  gap: 5%;
}

#tituloTabela {
  background-color: #f2f2f7;
  display: grid;
  align-items: center;
  gap: 2%;
  grid-template-columns: 0.2fr 1fr 2fr 1fr 1fr 2fr;
  /*0.2fr 0.4fr 1.8fr 1fr 1.3fr 2fr*/
  font-weight: bold;
  color: #9290a8;
  font-size: 0.6rem;
}

#tituloTabela p {
  margin: 0px 0px;
  width: 17%;
  text-align: left;
}

#linhasTabela {
  overflow-y: scroll;
  background-color: white;
  height: 40vh;
}

#barraEdicao {
  display: flex;
  justify-content: space-between;
  width: 50vw;
  align-items: center;
  /* margin-top: -2vh; */
}

#barraEdicao div {
  margin-inline: 2vw;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

#barraEdicao div:nth-child(1) {
  width: 30%;
}

#barraEdicao div:nth-child(2) {
  width: 20%;
}
</style>