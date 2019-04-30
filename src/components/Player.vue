<template>
  <div class="player">
    <draggable
      element="ul"
      class="list vList"
      :move="beforeMove"
      @end="onEnd"
      :list="Spaces" :options="{group:'card'}"
    >
      <li v-for="card in Spaces.Space1" class="card vCard">{{card.name}}</li>
    </draggable>
    <draggable
      element="ul"
      class="list vList"
      :move="beforeMove"
      @end="onEnd"
      :list="Spaces" :options="{group:'card'}"
    >
      <li v-for="card in Spaces.Space2" class="card vCard">{{card.name}}</li>
    </draggable>
    <draggable
      element="ul"
      class="list vList"
      :move="beforeMove"
      @end="onEnd"
      :list="Spaces" :options="{group:'card'}"
    >
      <li v-for="card in Spaces.Space3" class="card vCard">{{card.name}}</li>
    </draggable>
  </div>
</template>

<script>
import draggable from "vuedraggable";

import pick from "../utils/deck";
import calc from "../utils/calc";
import Card from "./Card";

export default {
  name: "dealer",
  components: { draggable, Card },
  data() {
    return {
      Spaces: {
        Space1: [{ name: "player" }, { name: "Appointment" }],
        Space2: [],
        Space3: [{ name: "3" }]
      }
    };
  },
  created: function() {
    this.hand.push(pick());

    this.hand[0].hide = true;

    this.$on("postexec", this.postexec);
  },
  methods: {
    postexec(playerBust) {
      this.hand[0].hide = false;
      // プレイヤーがBustしてない場合、17を超えるまでカードを引く
      while (!playerBust && calc(this.hand) < 17) {
        this.hand.push(pick());
      }
      this.$emit("result", calc(this.hand));
    },
    beforeMove: function(evt) {
      return evt.draggedContext.element.name !== "Meeting";
    },
    onEnd: function(evt) {
      console.log(evt);
    }
  }
};
</script>
