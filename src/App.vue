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
    <div id="nav" class="mb-3">
      <button class="btn btn-outline-secondary" @click="interface.page = 'workers'">Workers</button>
      <button class="btn btn-outline-secondary" @click="interface.page = 'upgrades'">Upgrades</button>
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
          <button @click="add_worker(worker.name)" class="btn btn-outline-dark mx-3">{{ workers_library[worker.name]}}</button>
          <span>{{ Math.floor(worker.calculate_price()) }}</span>
        </div>
      </template>

    </template>
    <template v-if="interface.page === 'upgrades'">
      <div class="row">
        <template v-for="upgrade in upgrades">
          <div class="col" v-if="upgrade.status === 'unlocked'">
            <button @click="buy_upgrade(upgrade)" class="btn btn-outline-dark">{{ upgrade.desc }}</button>
            <div class="upgrade-price">{{ upgrade.cost}}</div>
          </div>
        </template>
      </div>
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

      upgrades: [
        {
          worker: 'manual',
          type: 'additive',
          value: 2,
          name: '',
          desc: 'Each manual mines 2 ores more',
          cost: 50,
          status: 'unlocked'
        },
        {
          worker: 'manual',
          type: 'multiplicative',
          value: 10,
          name: '',
          desc: 'Make manual mining x10 effective',
          cost: 200,
          status: 'unlocked'
        }
      ],

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

    get_worker(worker_name) {
      return this.workers.find(worker => worker.name === worker.name);
    },

    buy_upgrade(upgrade) {
      let worker = this.get_worker(upgrade.worker);

      if (this.ore < upgrade.cost) return;

      if (upgrade.type === 'additive') {
        worker.add_additive_upgrade(upgrade.value);
      } else {
        worker.add_multiplicative_upgrade(upgrade.value);
      }

      this.ore -= upgrade.cost;
      upgrade.status = 'bought';
    },

    add_worker(worker_name, quantity = 1) {
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