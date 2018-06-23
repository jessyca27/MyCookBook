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
            Vous avez <a href="">{{ recipes.length }} recettes</a>
          </p>
          <p>
            <a href="#" class="btn btn-primary my-2">{{ nbEntree }}</a>
            <a href="#" class="btn btn-secondary my-2">{{ nbPlat }}</a>
            <a href="#" class="btn btn-success my-2">{{ nbDessert }}</a>
          </p>
        </div>
      </section>

      <div class="album py-5 bg-light">
        <div class="container">

          <div class="row">
            <div class="col-md-4" v-for="recipe in recipes">
              <div class="card mb-4 box-shadow">
                <img class="card-img-top" src="img/default-image.png" alt="Card image cap">
                <div class="card-body">
                  <h2>
                    {{ recipe.name }}
                    <span class="badge badge-primary" v-if="recipe.category === 'Entrée' ">Entrée</span>
                    <span class="badge badge-secondary" v-if="recipe.category === 'Plat' ">Plat</span>
                    <span class="badge badge-success" v-if="recipe.category === 'Dessert' ">Dessert</span>
                  </h2>
                  <p class="card-text">This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.</p>
                  <div class="d-flex justify-content-between align-items-center">
                    <div class="btn-group">
                      <button type="button" class="btn btn-sm btn-outline-secondary">View</button>
                      <button type="button" class="btn btn-sm btn-outline-secondary">Edit</button>
                    </div>
                    <small class="text-muted">9 mins</small>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-md-4">
              <div class="card mb-4 box-shadow">
                <div class="card-body">
                  <button type="button" @click="addRecipe">+</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>


    </main>

    <footer class="container">
      <p>&copy; Company 2017-2018</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
        username: 'Jessyca',
        recipes : [
            {
                id: 1,
                name: 'Spaghetti Bolognaise',
                category: 'Plat'
            },
            {
                id: 2,
                name: 'Poêlée asiatique',
                category: 'Plat'
            },
            {
                id: 3,
                name: 'Salade Lyonnaise',
                category: 'Entrée'
            },
            {
                id: 4,
                name: 'Compote pomme-banane',
                category: 'Dessert'
            }
        ]
    }
  },
    methods: {
        addRecipe: function () {
            let newRecipe = {
                name: 'Mousse au chocolat',
                category: 'Dessert'
            }
            this.recipes.push(newRecipe)
        }
    },
    computed: {
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
        }
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
