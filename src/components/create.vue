<template>
    <div class="pt-5">
        <form @submit.prevent="adicionar" method="post">
            <div class="form-group">
                <label for="nome">Nome</label>
                <input
                    required
                    type="text"
                    class="form-control"
                    id="nome"
                    v-model="produto.nome"
                    name="nome"
                    placeholder="Insira nome do produto"
                    :class="{'is-invalid': errors.has('produto.nome') && submitted}">
            </div>
            <div class="form-group">
                <label for="marca">Marca</label>
                <input
                    required
                    name="marca"
                    class="form-control"
                    id="marca"
                    v-model="produto.marca"
                    placeholder="Insira a marca"
                    :class="{'is-invalid': errors.has('produto.marca') && submitted}"/>
            </div>
            <div class="form-group">
                <label for="categoria">Categoria</label>
                <input
                    required
                    name="categoria"
                    class="form-control"
                    id="categoria"
                    v-model="produto.categoria"
                    placeholder="Insira uma categoria"
                    :class="{'is-invalid': errors.has('produto.categoria') && submitted}"/>
            </div>
            <div class="form-group">
                <label for="preco">Preço</label>
                 <input
                    required
                    type="number"
                    name="preco"
                    class="form-control"
                    id="preco"
                    step="0.01"
                    v-model="produto.preco"
                    placeholder="Insira o preço(somente números)"
                    :class="{'is-invalid': errors.has('produto.preco') && submitted}"/>
            </div>
            <button type="submit" class="btn btn-primary">Inserir</button>
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
    methods: {
        adicionar: function () {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.post('http://127.0.0.1:8000/produto/',this.produto).then(() => {
                        this.$router.push('/');
                    })
            });
        }
    },
}
</script>