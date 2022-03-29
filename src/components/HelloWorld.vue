<script>
const API_URL = `http://localhost:8000/`;

export default {
  data: () => ({
    // baseIngredients: ["chesse", "jam", "bacon", "peperoni", "tuna"],
    sizes: ["small", "medium", "large"],
    currentUrl: "pizza/",
    pizzas: null,
    ingredients: null,
    ingredientsOfOnePizza: null,
    allIngredients: null,
    allBases: null,
    ingredient: null,
    pizzafinished: [],

    checkedBaseIngredients: [],
    checkedIngredients: [],
    checkedBase: null,
    checkedSize: null,
    name: null,

    checkedBaseArray: [],
  }),

  created() {
    this.getPizzas();
    this.getAllIngredients();
    this.getAllBases();
  },

  watch: {
    currentBranch: "fetchData",
  },

  methods: {
    async showIngredientsOfOnePizza(id) {
      console.log(id);
      const url = `${API_URL}pizza/${id}/ingredients/`;
      this.ingredientsOfOnePizza = await (await fetch(url)).json();
    },
    async getAllIngredients() {
      const url = `${API_URL}pizza/ingredients/`;
      this.allIngredients = await (await fetch(url)).json();
    },
    async getAllBases() {
      const url = `${API_URL}pizza/bases/`;
      this.allBases = await (await fetch(url)).json();
      console.log(this.allBases.length);
    },
    async getPizzas() {
      const url = `${API_URL}${this.currentUrl}`;
      this.pizzas = await (await fetch(url)).json();
    },
    async checkForm() {
      let newBase = { base: this.checkedSize };
      let date = Date.now();
      let url = "http://localhost:8000/pizza/add/";
      let pizza = { name: this.name, pub_date: date };
      console.log("Estos son los ingredientes", this.checkedIngredients);
      console.log("Esta es la base de la base", this.checkedBaseIngredients);
      this.checkedBaseIngredients.forEach((element) => {
        this.checkedIngredients.push({ ingredient: element });
      });
      console.log("Esta es la base de la base 3", this.checkedIngredients);
      this.pizzafinished.push(pizza);
      this.pizzafinished.push(this.checkedIngredients);
      this.pizzafinished.push(newBase);
      console.log(this.pizzafinished);

      fetch(url, {
        method: "POST",
        body: JSON.stringify(this.pizzafinished),
        headers: { "Content-Type": "application/json" },
      })
        .then((res) => res.json())
        .catch((error) => console.error("Error:", error))
        .then((response) => console.log("Success:", response));
    },
  },
};
</script>

<template>
  <h1>The last 5 pizzas created</h1>
  <p>Select a pizza, and you can see the ingredients:</p>
  <template v-for="pizza in pizzas" :key="pizza.id">
    <input
      @click="showIngredientsOfOnePizza(pizza.id)"
      :for="pizza"
      type="radio"
      :id="pizza"
      :value="'pizza.name' + '/' + pizza.id + '/'"
      name="pizza"
      v-model="currentBranch"
    />
    <label> {{ pizza.name }}</label>
  </template>
  <ul>
    <li
      class="ingredient"
      v-for="ingredient in ingredientsOfOnePizza"
      :key="ingredient.id"
    >
      {{ ingredient.ingredient }}
    </li>
  </ul>
  <form id="app" @submit="checkForm">
    <label for="pizza">Name your pizza: </label>
    <input id="name" type="text" name="name" required v-model="name" />
    <br />

    <template v-for="ingrediente in allIngredients" :key="ingrediente.id">
      <input
        type="checkbox"
        :id="ingrediente.ingredient"
        :value="ingrediente"
        v-model="checkedIngredients"
      />
      <label :for="ingrediente.id"> {{ ingrediente.ingredient }}</label>
    </template>
    <br />
    <br />
    <div>Size of your pizza:</div>
    <div>
      <template v-for="size in sizes" :key="size.id">
        <label :for="size"> {{ size }}</label>
        <input
          type="radio"
          :id="size"
          :value="size"
          required
          v-model="checkedSize"
        />
      </template>
    </div>
    <!-- <div>
      <template v-for="base in allBases" :key="base.id">
        <input
          type="radio"
          :id="base.base"
          :value="base.base"
          required
          v-model="checkedBase"
        />
        <label :for="base.id"> {{ base.base }}</label>
      </template>
    </div> -->
    <p>
      <input type="submit" value="Enviar" />
    </p>
  </form>
</template>

<style>
.ingredient {
  list-style: none;
}
a {
  text-decoration: none;
  color: #42b883;
}
li {
  line-height: 1.5em;
  margin-bottom: 20px;
}
.author,
.date {
  font-weight: bold;
}
</style>
