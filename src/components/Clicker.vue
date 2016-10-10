<template>
  <div class="hello">
    <h1>Vue Cookie Clacker</h1>
    <div>
      <p>${{ cash }}</p>
      <img src="http://i.imgur.com/I9mLiI3.png"
        class="clicker"
        v-on:click="clicked"
        :style="'transform: rotate(' + clicks / 100 + 'deg)'"
      />
    </div>

    <div>
      <button
        v-for="(upgrade, i) in upgrades"
        v-on:click="buyUpgrade(upgrade)"
        :disabled="cash < upgradeCost(upgrade)"
        v-show="i === 0 || upgrades[i-1].count > 0"
      >Buy {{upgrade.name}} #{{upgrade.count+1}} - ${{ upgradeCost(upgrade) }}
      </button>

      <p>Clicks Per Second: {{ cps.toFixed(2) }}</p>
   </div>
  </div>
</template>

<script>
export default {

  data () {
    return {
      clicks: 0,
      cash: 0,
      cps: 0,

      upgrades: [
        {
          name: 'Cursor',
          base: 15,
          cps: 0.4,
          count: 0
        },
        {
          name: 'Grandma',
          base: 100,
          cps: 2,
          count: 0
        },

        {
          name: 'Farm',
          base: 1100,
          cps: 8,
          count: 0
        },

        {
          name: 'Mine',
          base: 12000,
          cps: 47,
          count: 0
        },
        {
          name: 'Factory',
          base: 130000,
          cps: 260,
          count: 0
        }
      ]
    }
  },

  methods: {
    addClicks: function (add) {
      this.cash = (this.cash + add).toFixed(2) * 1
      this.clicks += 1
    },
    autoClick: function () {
      var add = (this.cash + this.cps / 5).toFixed(2) * 1
      this.cash = add
      this.clicks += this.cps.toFixed(2) * 1
    },
    clicked: function () {
      this.addClicks(1)
    },
    buyUpgrade: function (upgrade) {
      this.cash = (this.cash - this.upgradeCost(upgrade)).toFixed(2) * 1
      this.cps += upgrade.cps
      upgrade.count += 1
    },
    upgradeCost: function (upgrade) {
      var cost = upgrade.base * Math.pow(1.15, upgrade.count)
      return Math.ceil(cost)
    },
    startAutoClickInterval: function () {
      setInterval(this.autoClick, 200)
    }
  },

  // computed: {

  // },

  created: function () {
    this.startAutoClickInterval()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  position: relative;
  z-index: 0;
}
button {
  position: relative;
  z-index: 1;
}
</style>
