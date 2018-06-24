<template>
  <div id="app">
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <a class="navbar-brand" href="#">Navbar</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExampleDefault" aria-controls="navbarsExampleDefault" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarsExampleDefault">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" href="#">Disabled</a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="http://example.com" id="dropdown01" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
            <div class="dropdown-menu" aria-labelledby="dropdown01">
              <a class="dropdown-item" href="#">Action</a>
              <a class="dropdown-item" href="#">Another action</a>
              <a class="dropdown-item" href="#">Something else here</a>
            </div>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="text" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
        </form>
      </div>
    </nav>

    <main role="main">

      <section class="jumbotron text-center mt-5">
        <div class="container">
          <h1 class="jumbotron-heading">Bonjour {{ username }}</h1>
          <p class="lead text-muted">
            Vous avez <a href="#" @click.prevent="filter('all')">{{ nbRecipes }}</a>
          </p>
          <p>
            <a href="#" class="btn btn-primary my-2" @click.prevent="filter('entree')">{{ nbEntree }}</a>
            <a href="#" class="btn btn-secondary my-2" @click.prevent="filter('plat')">{{ nbPlat }}</a>
            <a href="#" class="btn btn-success my-2" @click.prevent="filter('dessert')">{{ nbDessert }}</a>
          </p>
        </div>
      </section>

      <div class="album py-5 bg-light">
        <div class="container">

          <div class="row">
            <div class="col-8">
              <div class="row">

                <recipe-card
                    v-for="recipe in recipesFiltered"
                    :recipe="recipe"
                    @deleteRecipe="deleteRecipe"
                    @addToList="addToList"
                ></recipe-card>

                <div class="col-md-6">
                  <div class="card mb-4 box-shadow">
                    <div class="card-body">
                      <button type="button" @click="addRecipe">+</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-4">
              <h2>Liste de course</h2>

              <form @submit.prevent="addIngredient">
                <div class="form-row">
                  <div class="col-4">
                    <input type="text" class="form-control" placeholder="500g" v-model="quantity" required>
                  </div>
                  <div class="col-6">
                    <input type="text" class="form-control" placeholder="Farine" v-model="ingredient" required>
                  </div>
                  <div class="col-2">
                    <button type="submit" class="btn btn-primary mb-2"><i class="material-icons float-right">add</i></button>
                  </div>
                </div>
              </form>


              <ul class="list-group list-group-flush text-left">
                <li v-for="(ingredient, index) in list" :key="index" class="list-group-item">
                  {{ ingredient.quantity }} {{ ingredient.name }}
                  <a href="#" @click.prevent="deleteIngredient(ingredient)"><i class="material-icons float-right">close</i></a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>


    </main>

    <footer class="container">
      <p>&copy; Jessyca HERVIEUX 2018</p>
    </footer>
  </div>
</template>

<script>

  import RecipeCard from './Components/recipe-card.vue'

export default {
  name: 'app',
    components: {
        RecipeCard
    },
  data () {
    return {
        username: 'Jessyca',
        recipes : [
            {
                id: 1,
                name: 'Spaghetti Bolognaise',
                category: 'Plat',
                image: 'spaghetti-bolognaise.jpg',
                ingredients: [
                    {
                        name: 'spaghetti',
                        quantity: 500
                    },
                    {
                        name: 'viande hachée',
                        quantity: 250
                    },
                    {
                        name: 'purée de tomates',
                        quantity: 1
                    },
                    {
                        name: 'oignon',
                        quantity: 1
                    }
                ]
            },
            {
                id: 2,
                name: 'Poêlée asiatique',
                category: 'Plat',
                ingredients: [
                    {
                        name: 'spaghetti',
                        quantity: 500
                    },
                    {
                        name: 'carotte',
                        quantity: 2
                    },
                    {
                        name: 'poivron',
                        quantity: 1
                    },
                    {
                        name: 'blanc de poulet',
                        quantity: 800
                    }
                ]
            },
            {
                id: 3,
                name: 'Salade Lyonnaise',
                category: 'Entrée',
                image: 'salade-lyonnaise.jpg',
                ingredients: [
                    {
                        name: 'salade',
                        quantity: 1
                    },
                    {
                        name: 'lardons',
                        quantity: 200
                    },
                    {
                        name: 'tomate',
                        quantity: 2
                    }
                ]
            },
            {
                id: 4,
                name: 'Compote pomme-banane',
                category: 'Dessert',
                ingredients: [
                    {
                        name: 'pommes',
                        quantity: 5
                    },
                    {
                        name: 'banane',
                        quantity: 1
                    },
                    {
                        name: 'sucre vanillé',
                        quantity: 1
                    }
                ]
            }
        ],
        filtered: 'all',
        list: [
            {
                name: 'test',
                quantity: 1
            }
        ],
        ingredient: '',
        quantity: '',
        users: []
    }
  },
    methods: {
        addRecipe: function () {
            let newRecipe = {
                name: 'Mousse au chocolat',
                category: 'Dessert'
            }
            this.recipes.push(newRecipe)
        },
        filter: function (value) {
            this.filtered = value;
        },
        deleteRecipe: function (id) {
            this.recipes = this.recipes.filter(e => e.id != id)
            return this.recipes
        },
        addToList: function (ingredients) {
            let list = _.sortBy(this.list, ['name'])

            _.forEach(ingredients, function(ingredient) {
                if (_.find(list, {'name': ingredient.name}) != undefined ) {
                    let oldIngredient = _.find(list, {'name': ingredient.name})

                    let newIngredient = {
                        name: ingredient.name,
                        quantity: oldIngredient.quantity + ingredient.quantity
                    }

                    list.push(newIngredient)
                    _.remove(list, oldIngredient)
                } else {
                    list.push(ingredient)
                }
            });

            this.list = _.sortBy(list, ['name'])
        },
        deleteIngredient: function (ingredient) {
          this.list = this.list.filter(i => i != ingredient)
        },
        addIngredient: function () {
            this.addToList([
                {
                    name: this.ingredient.toLowerCase(),
                    quantity: parseInt(this.quantity)
                }
            ])

            this.ingredient = ''
            this.quantity = ''
        }
    },
    computed: {
        nbRecipes: function () {
            if (this.recipes.length > 1) {
                return this.recipes.length + ' recettes'
            }

            return this.recipes.length + ' recette';
        },
        nbEntree: function() {
            let nbEntree = _.filter(this.recipes, ['category', 'Entrée']);
            if (nbEntree.length > 1) {
                return nbEntree.length + ' entrées'
            }

            return nbEntree.length + ' entrée';
        },
        nbPlat: function() {
            let nbPlat = _.filter(this.recipes, ['category', 'Plat']);
            if (nbPlat.length > 1) {
                return nbPlat.length + ' plats'
            }

            return nbPlat.length + ' plat';
        },
        nbDessert: function() {
            let nbDessert = _.filter(this.recipes, ['category', 'Dessert']);
            if (nbDessert.length > 1) {
                return nbDessert.length + ' desserts'
            }

            return nbDessert.length + ' dessert';
        },
        recipesFiltered: function () {
            if (this.filtered === 'entree') {
                return _.filter(this.recipes, ['category', 'Entrée'])
            }
            if (this.filtered === 'plat') {
                return _.filter(this.recipes, ['category', 'Plat'])
            }
            if (this.filtered === 'dessert') {
                return _.filter(this.recipes, ['category', 'Dessert'])
            }

            return this.recipes
        }
    },
    mounted() {
      this.$http.get('http://jsonplaceholder.typicode.com/users')
          .then((response) => {
              this.users = response.data
          })
    }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
