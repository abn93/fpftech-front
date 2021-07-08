<template>
    <div class="pt-5">
        <form @submit.prevent="Editar" method="post">
            <div class="form-group">
                <label for="nome">Nome</label>
                <input
                    required
                    type="text"
                    class="form-control"
                    id="nome"
                    v-model="produto.nome"
                    name="nome"
                    placeholder="Editar nome"
                    :class="{'is-invalid': errors.has('produto.nome') && submitted}"/>
            </div>
            <div class="form-group">
                <label for="marca">Marca</label>
                <input
                    required
                    type="text"
                    name="marca"
                    class="form-control"
                    id="currency"
                    v-model="produto.marca"
                    placeholder="Editar marca"
                    :class="{'is-invalid': errors.has('produto.marca') && submitted}"/>
                
            </div>
            <div class="form-group">
                <label for="categoria">Categoria</label>
                <input
                    required
                    type="text"
                    name="categoria"
                    class="form-control"
                    id="categoria"
                    v-model="produto.categoria"
                    placeholder="Editar categoria"
                    :class="{'is-invalid': errors.has('produto.categoria') && submitted}"/>
            </div>
            <div class="form-group">
                <label for="produto">Preço</label>
                <input 
                    required
                    type="number"
                    name="preco"
                    class="form-control"
                    id="preco"
                    step="0.01"
                    v-model="produto.preco"
                    placeholder="Editar preço"
                    :class="{'is-invalid': errors.has('produto.preco') && submitted}"/>

            </div>
            <button type="submit" class="btn btn-primary">Editar</button>
        </form>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
                produto: {
                nome: '',
                marca: '',
                categoria: '',
                preco: '',
            },
            submitted: false
        }
    },
    mounted() {
        axios.get('http://127.0.0.1:8000/produto/' + this.$route.params.id + '/')
            .then( response => {
                this.produto = response.data
            });
    },
    methods: {
        Editar: function () {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                    
                }
                axios.put(`http://127.0.0.1:8000/produto/${this.$route.params.id}/`,this.produto,)
                .then(response => {
                    this.data = response.data,
                    this.$router.push('/')     
                    })
                    
            });
        }
    },
}
</script>