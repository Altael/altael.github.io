<template>
  <div class="container py-5">
    <div v-if="workers.length" id="info" class="row mb-3 mx-auto w-50">
      <div class="row">
        <div class="col">
          "Money":
        </div>
        <div class="col">
          {{ Math.floor(ore) }}
        </div>
      </div>
      <div class="row">
        <div class="col">
          Power of "mining":
        </div>
        <div class="col">
          {{ manual_worker.gather() }}
        </div>
      </div>
    </div>
    <div id="nav">
      <button class="btn btn-outline-secondary" @click="interface.page = 'workers'">Workers</button>
    </div>

    <template v-if="interface.page === 'workers'">
      <div id="controls" class="row">
        <div class="row mb-3">
          <button type="button" class="btn btn-dark btn-lg w-100" @click="ore += manual_worker.gather()">
            Mine ore... not yours. Get it?
          </button>
        </div>
      </div>
      <template v-for="worker in workers">
        <div>
          <img class="worker-img" :src="'../src/assets/img/worker_' + worker.name + '.png'" alt="">
          <button @click="upgrade(worker.name)" class="btn btn-outline-dark mx-3">{{ workers_library[worker.name]}}</button>
          <span>{{ Math.floor(worker.calculate_price()) }}</span>
        </div>
      </template>

    </template>
    <template v-if="interface.page === 'upgrades'">

    </template>
    <template v-if="interface.page === 'achievements'">

    </template>
    <template v-if="interface.page === 'statistics'">

    </template>
    <template v-if="interface.page === 'settings'">

    </template>

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
      interface: {
        page: 'workers'
      },

      workers: [],
      workers_library: {
        manual: 'Hand',
        venture: 'Venturae',
        retriever: 'Retriever',
        orca: 'Orca'
      },
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
      },

      tick_interval: 1000
    }
  },

  mounted() {
    this.workers_init();
    this.game_tick();

    this.fancy_rock_move();
  },

  methods: {
    game_tick() {
      this.workers.forEach(worker => {
        if (worker.name !== 'manual') {
          this.ore += worker.gather()
        }
      });

      setTimeout(() => {this.game_tick()}, this.tick_interval);
    },

    workers_init() {
      this.workers.push(new Worker('manual', 1, 10, 1.2, 1));
      this.workers.push(new Worker('venture', 100, 100, 1.2));
      this.workers.push(new Worker('retriever', 5000, 7500, 1.7));
      this.workers.push(new Worker('orca', 40000, 100000, 2.5));
    },

    upgrade(worker_name, quantity = 1) {
      let worker = this.workers.find(worker => {
        return worker.name === worker_name
      })

      let price = worker.calculate_price()

      if( this.ore >= price) {
        this.ore -= price;

        worker.add_quantity(quantity)
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
    manual_worker() {
      return this.workers.find(worker => {
        return worker.name === 'manual'
      });
    }
  }
}

</script>