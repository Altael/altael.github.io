<template>
  <div class="container py-5">
    <div id="info" class="mb-3 mx-auto w-50">
      <div class="row">
        <div class="col">
          "Money":
        </div>
        <div class="col">
          {{ ore }}
        </div>
      </div>
      <div class="row">
        <div class="col">
          Power of "mining":
        </div>
        <div class="col">
          {{ value(ore_gather_base) }}
        </div>
      </div>
    </div>
    <div id="controls" class="row">
      <div class="row mb-3">
        <button type="button" class="btn btn-dark btn-lg w-100" @click="ore_gather()">
          Mine ore... not yours. Get it?
        </button>
      </div>
      <div class="row">
        <div class="col d-flex flex-column align-items-center">
          <button type="button" class="btn btn-dark btn-lg w-100" @click="upgrade(ore_gather_base)">
            Upgrade your power (not actual power, just the number on top)
          </button>
          <div class="upgrade-price">{{ price(ore_gather_base) }} "money"</div>
        </div>
      </div>
    </div>

    <img @click="misc.fancy_rock.clicks++" src="../src/assets/img/veldspar.png" ref="fancy_rock" id="fancy-rock" :style="{
      top: misc.fancy_rock.top + 'px',
      left: misc.fancy_rock.left + 'px'
    }">
  </div>
</template>

<script>
export default {
  data() {
    return {
      money: 0,

      ore: 0,

      ore_gather_base: {
        level: 1,
        base_value: 1,
        increment_value: 1,
        base_price: 10,
        increment_price: 2
      },

      adds_base: [
        2
      ],

      mults: [
          2
      ],

      adds_general: [
        2
      ],

      misc: {
        fancy_rock: {
          top: 0,
          left: 0,
          vector_x: 1,
          vector_y: 1,
          clicks: 0
        }
      }
    }
  },

  mounted() {
    this.fancy_rock_move();
  },

  methods: {
    ore_gather() {
      let result = this.value(this.ore_gather_base);

      result = this.adds_base.reduce( (gather, add) => {
        return gather + add
      }, result);

      result = this.mults.reduce( (gather, mult) => {
        return gather * mult
      }, result);

      result = this.adds_general.reduce( (gather, add) => {
        return gather + add
      }, result);

      this.ore += result;
    },

    upgrade(param) {
      let price = this.price(param);

      if( this.ore >= price) {
        this.ore -= price;

        param.level++;
      }
    },

    value(param) {
      return param.base_value + (param.level - 1) * param.increment_value;
    },

    price(param) {
      return param.base_price + (param.level - 1) * param.increment_price;
    },

    fancy_rock_move() {
      this.misc.fancy_rock.top += this.misc.fancy_rock.vector_y;
      this.misc.fancy_rock.left += this.misc.fancy_rock.vector_x;

      if (this.misc.fancy_rock.top >= (window.innerHeight - this.$refs.fancy_rock.scrollHeight) ||
          this.misc.fancy_rock.top <= 0) {
        this.misc.fancy_rock.vector_y *= -1
      }

      if (this.misc.fancy_rock.left >= (window.innerWidth - this.$refs.fancy_rock.scrollWidth) ||
          this.misc.fancy_rock.left <= 0) {
        this.misc.fancy_rock.vector_x *= -1
      }

      setTimeout(() => {
        this.fancy_rock_move();
      }, 10);
    }
  },

  computed: {

  }
}

</script>