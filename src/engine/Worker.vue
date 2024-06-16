<script>

export class Worker {

    /**
     * 
     * @param base_value gathered currency by this worker per tick
     * @param quantity number of workers of this type
     * @param upgrade_additive additive upgrades to a worker (e.g. +100 and +200 upgrades will be [100, 200])
     * @param upgrade_multiplicative multipricative upgrades to a worker (e.g. has 2 upgrades: 20% and 30% will be array [1.2, 1.3])
     */
    constructor(base_value, quantity = 1, upgrade_additive = [], upgrade_multiplicative = []) {
        this.base_value = base_value
        this.quantity = quantity
        this.upgrade_additive = upgrade_additive
        this.upgrade_multiplicative = upgrade_multiplicative
    }

    tick() {
      let result = this.base_value * this.quantity

      result = this.upgrade_additive.reduce( (accumulator, add) => {
        return accumulator + add
      }, result)

      result = this.upgrade_multiplicative.reduce( (accumulator, mult) => {
        return accumulator * mult
      }, result);

      return result; 
    }

    increase_quantity(quantity) {
        this.quantity += quantity
    }

    add_additive_upgrade(upgrade_value) {
        this.upgrade_additive.push(upgrade_value)
    }

    add_multiplicative_upgrade(upgrade_multiplier) {
        this.upgrade_multiplicative.push(upgrade_multiplier)
    }
}

</script>
