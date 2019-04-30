<template>
  <div class="game">
    <span>-Arena erea</span>
    <dealer @stand="stand" :showButtons="showButtons"/>
    <div class="message result spacer">{{ resultMessage }}</div>
  </div>
</template>

<script>
import Dealer from "./Dealer";
import Player from "./Player";

export default {
  name: "game",
  components: { Dealer, Player },
//  components: { Dealer },
  data() {
    return {
      mainMessage: "Welcome to Game",
      playersResult: 0,
      dealersResult: 0,
      showButtons: true,
    };
  },
  methods: {
    stand: function(playersResult) {
      this.playersResult = playersResult;
      this.$refs.dealer.$emit("postexec", playersResult === "Bust");
    },
    postexec: function(dealersResult) {
      this.dealersResult = dealersResult;
      this.showButtons = false;
      this.mainMessage = `Dealer : ${dealersResult} / Player : ${
        this.playersResult
      }`;
    }
  },
  computed: {
    resultMessage: function() {
      if (this.showButtons) {
        return "";
      }
      if (
        this.playersResult > this.dealersResult ||
        this.dealersResult === "Bust"
      ) {
        return "You Win";
      }
      if (
        this.playersResult < this.dealersResult ||
        this.playersResult === "Bust"
      ) {
        return "You Lose";
      }
      return "Draw";
    }
  }
};
</script>


<style>
.flexContainer {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: flex-start;
  /** justify-content: space-between; **/
  align-items: stretch;
  align-content: stretch;
}

.spacer {
  padding-bottom: 40px;
}

div.dealer,
div.player {
  display: flex;
  justify-content: space-around; /*均等に間隔をあける・両端にも間隔をあける*/

  padding: 8px;
  margin-bottom: 8px;
  width: 800px;
  border: 1px solid #222;
  border-radius: 4px;
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.2);
}
div.are,
notuse {
  padding: 8px;
  margin: 8px;
  width: 150px;
  border: 1px solid #222;
  border-radius: 4px;
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.2);
}

div.deale,
notuse {
  display: flex;
  justify-content: space-around; /*均等に間隔をあける・両端にも間隔をあける*/

  padding: 8px;
  margin-bottom: 8px;
  width: 800px;
  border: 1px solid #222;
  border-radius: 4px;
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.2);
}

#ap,
notuse {
  display: flex;
  justify-content: space-around;
}
.vList {
  witdh: 150px;
  height: 300px;
  list-style: none;
  padding: 20px;
  margin: 0px;
}
.vCard {
  padding: 10px;
  margin-bottom: 10px;
}
.material,
.list,
.card {
  border: 1px solid #222;
  border-radius: 8px;
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.2);
}
.card {
  witdh: 100px;
  /** background-color: #fff; ***/
  cursor: grab;
}
.dragarea {
  witdh: 150px;
  height: 300px;
  position: relative;
  z-index: 99999;
}
</style>