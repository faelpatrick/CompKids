<template>
    <v-card class="conteudo" elevation="2">

        <!-- <h1 class="d-flex align-center justify-center">Jogo Compara Peças Pc</h1> -->

        <v-row>
            <v-col class="d-flex text-center align-center justify-space-between ma-2">
                <v-card width="100" class="pa-4 d-flex align-center justify-center flex-column" color="red">
                    Erros
                    <span class="placar"> {{ erros }}</span>
                </v-card>
                <v-card class="pa-4">
                    <h1 class="placar_titulo"> Escolha a opção correta</h1>
                </v-card>
                <v-card width="100" class="pa-4 d-flex align-center justify-center flex-column" color="green">Acertos
                    <span class="placar"> {{ acertos }}</span>
                </v-card>

            </v-col>
        </v-row>

        <v-row class="grid_imgs">
            <v-col class="cards_principal">
                <div>
                    <!-- <v-img height="300" src="/pecas/ram.webp" /> -->
                    <h1 class="align-center text-center">{{ imgPrincipal.Nome }}</h1>
                    <img id="img_principal" :src="imgPrincipal.ImgPeca" />
                    <p class="img_desc">{{ imgPrincipal.Desc }}</p>
                </div>
            </v-col>
            <v-col class="cards_opcoes">
                <v-card class="card pa-1" v-for="pecas in pecasComputador" :key="pecas.id" @click="checkAssociation">
                    <img class="img_opcao" :src="pecas.ImgCompara" />
                </v-card>
            </v-col>
        </v-row>

        <v-snackbar v-model="mostrarSnackbar" :timeout="3000" bottom right>
            {{ snackBarMsg }}
            <template v-slot:action="{ attrs }">
                <v-btn color="pink" text v-bind="attrs" @click="mostrarSnackbar = false">
                    Fechar
                </v-btn>
            </template>
        </v-snackbar>
    </v-card>
</template>
  
<script setup>
import { onMounted, ref } from 'vue';

const mostrarSnackbar = ref(false);
let snackBarMsg = ref('Ação realizada com sucesso!!!');
let imgPrincipal = ref({});

const pecasComputador = ref([
    {
        id: '01',
        Nome: "CPU",
        Compara: "Cérebro",
        ImgPeca: "/pecas/cpu.webp",
        ImgCompara: "/pecas/brain.webp",
        Desc: "Assim como o cérebro processa informações e toma decisões, a CPU processa instruções e executa operações lógicas e aritméticas."
    },
    {
        id: '02',
        Nome: "Memória RAM",
        Compara: "Memória de Curto Prazo",
        ImgPeca: "/pecas/ram.webp",
        ImgCompara: "/pecas/memoria_curto_prazo.webp",
        Desc: "A RAM armazena dados temporariamente para acesso rápido, similar à memória de curto prazo humana que retém informações temporariamente para tarefas imediatas."
    },
    {
        id: '03',
        Nome: "Disco Rígido/SSD",
        Compara: "Memória de Longo Prazo",
        ImgPeca: "/pecas/ssd.webp",
        ImgCompara: "/pecas/memoria_longo_prazo.webp",
        Desc: "Estas unidades armazenam dados de forma permanente, semelhante à memória de longo prazo que guarda informações por um longo período."
    },
    {
        id: '04',
        Nome: "Placa-mãe",
        Compara: "Sistema Circulatório",
        ImgPeca: "/pecas/placa_mae.webp",
        ImgCompara: "/pecas/cidade.webp",
        Desc: "A placa-mãe distribui energia e permite a comunicação entre todos os componentes do computador, assim como o sistema circulatório distribui sangue e nutrientes pelo corpo."
    },
    {
        id: '05',
        Nome: "Fonte de Alimentação",
        Compara: "Coração",
        ImgPeca: "/pecas/fonte_alimentacao.webp",
        ImgCompara: "/pecas/coracao.webp",
        Desc: "A fonte de alimentação fornece energia para todos os componentes do computador, da mesma forma que o coração bombeia sangue para fornecer energia ao corpo."
    },
    {
        id: '06',
        Nome: "GPU",
        Compara: "Olhos",
        ImgPeca: "/pecas/gpu.webp",
        ImgCompara: "/pecas/olhos.webp",
        Desc: "A GPU é responsável por renderizar imagens e vídeos, facilitando a interação visual com o computador, assim como os olhos permitem a visão e a interpretação visual do mundo."
    },
    {
        id: '07',
        Nome: "Teclado",
        Compara: "Lápis/Caneta",
        ImgPeca: "/pecas/teclado.webp",
        ImgCompara: "/pecas/lapis_caneta.webp",
        Desc: "O teclado permite a entrada de dados e comandos, similarmente a como um lápis ou caneta é utilizado para escrever informações."
    },
    {
        id: '08',
        Nome: "Mouse",
        Compara: "Mão",
        ImgPeca: "/pecas/mouse.webp",
        ImgCompara: "/pecas/mao.webp",
        Desc: "O mouse é usado para navegar e interagir com o ambiente digital, de forma análoga à mão, que interage com objetos no ambiente físico."
    },
    {
        id: '09',
        Nome: "Placa de Rede",
        Compara: "Sistema de Comunicação",
        ImgPeca: "/pecas/placa_rede.webp",
        ImgCompara: "/pecas/sistema_comunicacao.webp",
        Desc: "A placa de rede permite a comunicação do computador com redes e outros dispositivos, assemelhando-se à capacidade humana de comunicar-se através de linguagem e sinais."
    },
    {
        id: '10',
        Nome: "Cooler/Ventoinha",
        Compara: "Sistema de Refrigeração/Suor",
        ImgPeca: "/pecas/cooler.webp",
        ImgCompara: "/pecas/sistema_refrigeracao.webp",
        Desc: "Assim como o suor ajuda a regular a temperatura do corpo, os coolers ajudam a manter os componentes do computador em uma temperatura operacional segura."
    }
]);
const acertos = ref(0);
const erros = ref(0);

onMounted(() => {
    mostrarSnackbar.value = false;
    shuffle();
});

const checkAssociation = () => {
    if (pecasComputador[0].Compara === pecasComputador[0].Nome) {
        acertos.value++;
        snackBarMsg.value = 'Parabéns, você acertou!!!';
    } else {
        erros.value++;
        snackBarMsg.value = 'Que pena, você errou!!!';
    }
    mostrarSnackbar.value = true;
};

const shuffle = () => {
    pecasComputador.value.sort(() => Math.random() - 0.5);
    imgPrincipal = pecasComputador.value[0];
};

</script>
  
<style scoped>
.conteudo {
    min-height: 90vh;
}

.cards_principal {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 1px;
    margin: 0 8px 0 2rem;
    padding: 1rem 2rem;
    background-color: #ededed;
    border-radius: 10px;
    border: solid 2px #58585870;
}

.cards_opcoes {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    margin: 2px;
    gap: 4px;
}



.img_opcao {
    width: 100%;
    height: 200px;
    margin: 4px;
    border-radius: 16px;
    filter: drop-shadow(0px 2px 2px rgba(0, 0, 0, 0.25));
    border: solid 2px #000;
}

#img_principal {
    width: 350px;
}

.img_desc {
    font-size: 1.4rem;
    padding: 1rem;
    line-height: 2;
    text-align: justify;
}

.placar {
    display: block;
    font-size: 50px;
}

.v-row.grid_imgs {
    display: grid;
    grid-template-columns: 1fr 2fr;
}

@media screen and (max-width: 1024px) {
    .conteudo {
        margin: -20px;
    }

    .v-row.grid_imgs {
        display: flex;
        flex-direction: column;
    }

    .cards_principal {
        margin: 0;
    }

    .cards_opcoes {
        margin: 0;
        padding: 2px
    }

    #img_principal {
        width: 94%;
    }

    .img_opcao {
        height: 140px;
    }

    .img_desc {
        font-size: 1rem;
        line-height: 1.2;
    }

    .placar_titulo {
        font-size: 1.2rem;
    }

}</style>