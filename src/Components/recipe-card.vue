<template>
    <div class="col-md-6">
        <div class="card mb-4 box-shadow">
            <img :src="'img/' + recipe.image" :alt="recipe.name" :title="recipe.name" v-if="recipe.image" class="card-img-top">
            <img class="card-img-top" src="img/default-image.png" :alt="recipe.name" :title="recipe.name" v-else>
            <div class="card-body">
                <h2>
                    {{ recipe.name }}
                    <span class="badge badge-primary" v-if="recipe.category === 'Entrée' ">Entrée</span>
                    <span class="badge badge-secondary" v-if="recipe.category === 'Plat' ">Plat</span>
                    <span class="badge badge-success" v-if="recipe.category === 'Dessert' ">Dessert</span>
                </h2>
                <div class="card-text">
                    <ul class="list-group list-group-flush text-left">
                        <li v-for="ingredient in recipe.ingredients" class="list-group-item">{{ ingredient.quantity }} {{ ingredient.name }}</li>
                    </ul>
                </div>
                <div class="d-flex justify-content-between align-items-center">
                    <div class="btn-group">
                        <button type="button" class="btn btn-sm btn-outline-secondary">Voir</button>
                        <button type="button" class="btn btn-sm btn-outline-secondary">Editer</button>
                        <button type="button" class="btn btn-sm btn-outline-success" @click="addToList(recipe.ingredients)">Ajouter à la liste</button>
                        <button type="button" class="btn btn-sm btn-outline-danger" @click="deleteRecipe(recipe)">Supprimer</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            recipe: Object
        },
        methods: {
            deleteRecipe(recipe) {
                this.$emit('deleteRecipe', recipe.id)
            },
            addToList: function(ingredients) {
                this.$emit('addToList', ingredients)
            }
        }
    }
</script>