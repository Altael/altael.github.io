<template>
  <div class="container py-5">
    <div id="info" class="mb-5">
      <div class="row w-50 me-auto ">
        <div class="col align-self-center">
          "Money":
        </div>
        <div class="col align-self-center">
          {{ money }}
        </div>
      </div>
      <div class="row w-50 me-auto ">
        <div class="col align-self-center">
          Power of "mining":
        </div>
        <div class="col align-self-center">
          {{ value(click_increment) }}
        </div>
      </div>
    </div>
    <div id="controls" class="row">
      <div class="col">
        <button type="button" class="btn btn-outline-dark btn-lg w-100" @click="increment(click_increment)">
          Mine ore... not yours. Get it?
        </button>
      </div>
      <div class="col">
        <button type="button" class="btn btn-outline-dark btn-lg w-100" @click="upgrade(click_increment)">
          Upgrade your power (not actual power, just the number on top)
        </button>
        {{ price(click_increment) }} "money"
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      money: 0,
      click_increment: {
        level: 1,
        base_value: 1,
        increment_value: 1,
        base_price: 10,
        increment_price: 2
      }
    }
  },

  mounted() {

  },

  methods: {
    increment(param) {
      this.money += this.value(param);
    },

    upgrade(param) {
      let price = this.price(param);

      if( this.money >= price) {
        this.money -= price;

        param.level++;
      }
    },

    value(param) {
      return param.base_value + (param.level - 1) * param.increment_value;
    },

    price(param) {
      return param.base_price + (param.level - 1) * param.increment_price;
    }
  }
}

</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
