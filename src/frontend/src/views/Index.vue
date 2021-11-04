<template>
  <div>
    <header class="header">
      <div class="header__logo">
        <a href="index.html" class="logo">
          <img
            src="@/assets/img/logo.svg"
            alt="V!U!E! Pizza logo"
            width="90"
            height="40"
          />
        </a>
      </div>
      <div class="header__cart">
        <a href="cart.html">0 ₽</a>
      </div>
      <div class="header__user">
        <a href="#" class="header__login"><span>Войти</span></a>
      </div>
    </header>
    <main class="content">
      <form action="#" method="post">
        <div class="content__wrapper">
          <h1 class="title title--big">Конструктор пиццы</h1>
          <div class="content__dough">
            <div class="sheet">
              <h2 class="title title--small sheet__title">Выберите тесто</h2>

              <div class="sheet__content dough">
                <label
                  v-for="(dough, id) in preparedDough"
                  :key="`dough-${id}`"
                  class="dough__input"
                  :class="{
                    'dough__input--light': dough.name === 'Тонкое',
                    'dough__input--large': dough.name === 'Толстое',
                  }"
                >
                  <input
                    type="radio"
                    name="dought"
                    class="visually-hidden"
                    :checked="id === 0"
                    :value="dough.value"
                  />
                  <b>{{ dough.name }}</b>
                  <span>{{ dough.description }}</span>
                </label>
              </div>
            </div>
          </div>
          <div class="content__diameter">
            <div class="sheet">
              <h2 class="title title--small sheet__title">Выберите размер</h2>
              <div class="sheet__content diameter">
                <label
                  v-for="(size, id) in preparedSizes"
                  :key="`size-${id}`"
                  class="diameter__input"
                  :class="`diameter__input--${size.value}`"
                >
                  <input
                    class="visually-hidden"
                    type="radio"
                    name="diameter"
                    :value="size.value"
                    :checked="id === 0"
                  />
                  <span>{{ size.name }}</span>
                </label>
              </div>
            </div>
          </div>
          <div class="content__ingredients">
            <div class="sheet">
              <h2 class="title title--small sheet__title">
                Выберите ингредиенты
              </h2>
              <div class="sheet__content ingredients">
                <div class="ingredients__sauce">
                  <p>Основной соус:</p>
                  <label
                    v-for="(souse, id) in preparedSauces"
                    :key="`sourse-${id}`"
                    class="radio ingredients__input"
                  >
                    <input
                      type="radio"
                      name="sauce"
                      :value="souse.value"
                      :checked="id === 0"
                    />
                    <span>{{ souse.name }}</span>
                  </label>
                </div>
                <div class="ingredients__filling">
                  <p>Начинка:</p>
                  <ul class="ingredients__list">
                    <li
                      v-for="(ingredient, id) in pizza.ingredients"
                      :key="`ingredients-${id}`"
                      class="ingredients__item"
                    >
                      <span
                        class="filling"
                        :class="{
                          'filling--mushrooms': ingredient.name === 'Грибы',
                          'filling--cheddar': ingredient.name === 'Чеддер',
                          'filling--salami': ingredient.name === 'Салями',
                          'filling--ham': ingredient.name === 'Ветчина',
                          'filling--ananas': ingredient.name === 'Ананас',
                          'filling--bacon': ingredient.name === 'Бекон',
                          'filling--onion': ingredient.name === 'Лук',
                          'filling--chile': ingredient.name === 'Чили',
                          'filling--jalapeno': ingredient.name === 'Халапеньо',
                          'filling--olives': ingredient.name === 'Маслины',
                          'filling--tomatoes': ingredient.name === 'Томаты',
                          'filling--salmon': ingredient.name === 'Лосось',
                          'filling--mozzarella':
                            ingredient.name === 'Моцарелла',
                          'filling--parmesan': ingredient.name === 'Пармезан',
                          'filling--blue_cheese': ingredient.name === 'Блю чиз',
                        }"
                        >{{ ingredient.name }}</span
                      >
                      <div class="counter counter--orange ingredients__counter">
                        <button
                          type="button"
                          class="counter__button counter__button--minus"
                          disabled
                        >
                          <span class="visually-hidden">Меньше</span>
                        </button>
                        <input
                          type="text"
                          name="counter"
                          class="counter__input"
                          value="0"
                        />
                        <button
                          type="button"
                          class="counter__button counter__button--plus"
                        >
                          <span class="visually-hidden">Больше</span>
                        </button>
                      </div>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
          <div class="content__pizza">
            <label class="input">
              <span class="visually-hidden">Название пиццы</span>
              <input
                type="text"
                name="pizza_name"
                placeholder="Введите название пиццы"
              />
            </label>
            <div class="content__constructor">
              <div class="pizza pizza--foundation--big-tomato">
                <div class="pizza__wrapper">
                  <div class="pizza__filling pizza__filling--ananas"></div>
                  <div class="pizza__filling pizza__filling--bacon"></div>
                  <div class="pizza__filling pizza__filling--cheddar"></div>
                </div>
              </div>
            </div>
            <div class="content__result">
              <p>Итого: 0 ₽</p>
              <button type="button" class="button" disabled>Готовьте!</button>
            </div>
          </div>
        </div>
      </form>
    </main>
  </div>
</template>
<script>
import misc from "@/static/misc.json";
import pizza from "@/static/pizza.json";
import user from "@/static/user.json";
export default {
  name: "Index",
  data() {
    return {
      misc,
      pizza,
      user,
    };
  },
  computed: {
    preparedDough() {
      return this.pizza.dough.map((dough) => {
        let value = "";
        switch (dough.name) {
          case "Тонкое":
            value = "light";
            break;
          case "Толстое":
            value = "large";
            break;
          default:
            value = "default";
        }
        return {
          ...dough,
          value,
        };
      });
    },
    preparedSizes() {
      return this.pizza.sizes.map((size) => {
        let value = "";
        switch (size.multiplier) {
          case 1:
            value = "small";
            break;
          case 2:
            value = "normal";
            break;
          case 3:
            value = "big";
            break;
          default:
            value = "default";
        }
        return {
          ...size,
          value,
        };
      });
    },
    preparedSauces() {
      return this.pizza.sauces.map((sauce) => {
        let value = "";
        switch (sauce.name) {
          case "Томатный":
            value = "tomato";
            break;
          case "Сливочный":
            value = "creamy";
            break;
          default:
            value = "default";
        }
        return {
          ...sauce,
          value,
        };
      });
    },
  },
};
</script>

<!-- <style scoped></style> -->
