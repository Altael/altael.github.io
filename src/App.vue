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

import { Worker } from "./engine/Worker.vue"

export default {
  data() {
    return {
      manual_worker: new Worker(),
      workers: [],
      money: 0,
      ore: 0,

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
      this.ore += this.manual_worker.tick();
    },

    upgrade(param) {
      let price = this.manual_worker.check_price()

      if( this.ore >= price) {
        this.ore -= price;

        this.manual_worker.add_quantity(1)
      }
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