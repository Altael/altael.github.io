<script>

export class Worker {

    /**
     * 
     * @param name workers unique name
     * @param base_gather gathered currency per tick
     * @param quantity number of workers
     * @param upgrade_additive additive upgrades (e.g. +100 and +200 upgrades will be [100, 200])
     * @param upgrade_multiplicative multiplicative upgrades (e.g. has 2 upgrades: 20% and 30% will be array [1.2, 1.3])
     * @param base_price price of a new worker
     * @param price_multiplier 1 if new worker costs same. >1 if each subsequent worker is more expensive
     */
    constructor(name, base_gather, base_price, price_multiplier, quantity = 0, upgrade_additive = [], upgrade_multiplicative = []) {
        this.name = name

        this.base_gather = base_gather
        this.base_price = base_price
        this.price_multiplier = price_multiplier

        this.quantity = quantity

        this.upgrade_additive = upgrade_additive
        this.upgrade_multiplicative = upgrade_multiplicative
    }

    gather() {
      let result = this.base_gather

      result = this.upgrade_additive.reduce( (accumulator, add) => {
        return accumulator + add
      }, result)

      result *= this.quantity

      result = this.upgrade_multiplicative.reduce( (accumulator, mult) => {
        return accumulator * mult
      }, result)

      return result
    }

    /**
     * @param quantity number of workers to add
     */
    add_quantity(quantity) {
        this.quantity += quantity
    }

    add_additive_upgrade(upgrade_value) {
        this.upgrade_additive.push(upgrade_value)
    }

    add_multiplicative_upgrade(upgrade_multiplier) {
        this.upgrade_multiplicative.push(upgrade_multiplier)
    }

    /**
     * calculates price of an additional worker
     */
     calculate_price() {
        return this.base_price * Math.pow(this.price_multiplier, this.quantity)
    }
}

</script>
