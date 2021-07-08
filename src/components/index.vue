<template>
    <div class="pt-5">
        <div v-if="produtos && produtos.length">
            <div class="card mb-3" v-for="produto of produtos" v-bind:key="produto.id">
                <div class="row no-gutters">
                    <div class="col-md-8">
                        <div class="card-body">
                            <h5 class="card-title">{{ produto.nome }}</h5>
                            <p class="card-text">
                                {{'Nome produto: ' + produto.nome}},
                                {{'Marca: ' + produto.marca}}, 
                                {{'Categoria: ' + produto.categoria}},
                                {{'Preço: ' + produto.preco + ' R$'}}
                            </p>
                            <router-link :to="{name: 'edit', params: { id: produto.id }}" 
                            class="btn btn-sm btn-primary">Editar</router-link>

                            <button class="btn btn-danger btn-sm ml-1"
                             v-on:click="deleteProduto(produto)">Deletar</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <p  v-if="produtos.length == 0">Sem produtos até o momento</p>
    </div>
</template>
<script>

import axios from 'axios';
import swal  from 'sweetalert';
export default {
    data() {
        return {
            produtos: []
        }
    },
    created() {
        
        this.all();
    },
    methods: {
        deleteProduto: function(produto) {
           swal({
				title: 'Tem certeza?',
				text: 'Uma vez deletado, você não conseguirá recuperar o registro!',
				icon: 'warning',
				buttons: true,
				dangerMode: true,
			}).then((willDelete) => {
				if (willDelete) {
					axios.delete(`http://127.0.0.1:8000/produto/${produto.id}/`).then(() => {
						this.all();
					});
					swal('O seu registro foi deletado', {
						icon: 'success',
					});
				} else {
					swal('Registro não deletado');
				}
			});   
        },
        all: function () {
            axios.get('http://127.0.0.1:8000/produto/').then( response => {
                this.produtos = response.data
                console.log(response)
                });
        }
    },
}
</script>


