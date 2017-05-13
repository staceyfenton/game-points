<template>
  <div id="app">
    <header>
      <h1>
        <img src="./assets/img/kahoot.svg" alt="Kahoot!">
        <span>Points</span>
      </h1>
    </header>

    <main>
      <section class="game">
        <h2>Items</h2>
         <div class="items">
          <button v-for="(item, index) in items" @click="collectItem" :class="item.cssClass" :data-index="index">{{ item.name }}</button>
        </div>
      </section>

      <div class="scoreboard">
        <h2>Player items</h2>
        <p class="no-items" v-if="!sortedItems.length">You don't have any items yet.<br>Press an item to collect it.</p>
        <table v-if="sortedItems.length">
          <thead>
            <tr>
              <th>Item</th>
              <th>Qty</th>
              <th>Score</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in sortedItems">
              <td><span :class="item.cssClass">{{ item.name }}</span></td>
              <td>{{ item.quantity }}</td>
              <td>{{ item.totalScore }}</td>
            </tr>
          </tbody>
        </table>

        <div class="bonuses" v-if="sortedItems.length">
          Bonuses: {{ bonuses }}
        </div>

        <div class="score-summary">
          <div class="total" v-if="sortedItems.length">
            Total: {{ total }}
          </div>
          <button @click="resetGame">New game</button>
        </div>    
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
      return {
        items: [
          {
            name: 'A',
            basePoints: 50,
            bonusPoints: 0,
            bonusTrigger: 3,
            quantity: 0,
            totalScore: 0,
            cssClass: 'item-a'
          },
          {
            name: 'B',
            basePoints: 30,
            bonusPoints: 0,
            bonusTrigger: 2,
            quantity: 0,
            totalScore: 0,
            cssClass: 'item-b'
          },
          {
            name: 'C',
            basePoints: 20,
            bonusPoints: 0,
            quantity: 0,
            totalScore: 0,
            cssClass: 'item-c'
          },
          {
            name: 'D',
            basePoints: 15,
            bonusPoints: 0,
            quantity: 0,
            totalScore: 0,
            cssClass: 'item-d'
          }
        ],
        bonuses: 0,
        total: 0
      }
    },

    computed: {
      sortedItems() {
        const filterScores = this.items.filter(item => item.totalScore > 0);
        return filterScores.sort((a, b) => a.totalScore < b.totalScore ? 1 : -1);
      }
    },

    methods: {
      collectItem(event) {
        const currentItem = this.items[event.target.dataset.index];

        currentItem.quantity = currentItem.quantity + 1;
        
        if(currentItem.bonusTrigger) {          
          // has the bonus been triggered?
          const bonuses = Math.floor(currentItem.quantity / currentItem.bonusTrigger);
          if(bonuses >= 1) {
            currentItem.bonusPoints = bonuses * currentItem.basePoints
          }
          
          // update score
          currentItem.totalScore = (currentItem.basePoints * currentItem.quantity) + currentItem.bonusPoints;

          // update bonus total
          this.bonuses = this.calculateBonuses();

        } else {
          // no bonus points, just update the total
          currentItem.totalScore = currentItem.basePoints * currentItem.quantity;
        }

        // update grand total 
        this.total = this.calculateTotal();
      },

      calculateTotal() {
        const totals = this.items.map(item => item.totalScore);
        return totals.reduce((a, b) => a + b, 0);
      },

      calculateBonuses() {
        const bonuses = this.items.map(item => item.bonusPoints);
        return bonuses.reduce((a, b) => a + b, 0);
      },
    
      resetGame() {
        this.items.forEach(item => {
          item.quantity = 0;
          item.totalScore = 0;
          item.bonusPoints = 0;
        });
        this.bonuses = 0;
        this.total = 0;
      }
    }
  }
</script>
