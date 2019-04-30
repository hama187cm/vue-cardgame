<template>
  <div class="dealer">
    <v-container fluid grid-list-xl>
      <v-layout row wrap justify-center>
        <v-flex xs3 ma-2 pa-0
              v-for="(space, index) in Spaces" v-bind:key="index"
        >
          <div class="list vList">
            <draggable :list="Spaces" :group='Spaces' :animation=300
              :move="beforeMove" @end="onEnd"
            >
                <v-card dark color="primary" class="pa-2 ma-2 card" width="150px"
                  v-for="(card, index) in space" v-bind:key="index"
                  >
                    <span v-if="card.name != 'Card drag area'">{{card.name}}</span>
                    <span v-else>{{card.name}}</span>
                </v-card>
            </draggable>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
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
      Spaces: [
        [
          { name: "Card drag area" },
          { name: "areana" },
          { name: "Appointment" }
        ],
        [{ name: "Card drag area" }],
        [{ name: "Card drag area" }, { name: "3" }]
      ],
      SpaceNullFags: []
    };
  },
  watch: {
    dealerId: {
      handler() {
        console.log("Space1");
      },
      deep: true
    }
  },
  /*** 
  watch: {
    'Spaces.Space1': function(){
        if( Spaces.Space1.length == 0){ SpaceNullFags.push( "Space1" ); console.log( "Space1" ); }
        console.log( "other" );
    },
  },***/
  /*** 
  created: function() {
    //this.hand.push(pick());
    this.hand[0].hide = true;
    this.$on("postexec", this.postexec);
  },***/
  methods: {
    postexec(playerBust) {
      this.hand[0].hide = false;
      // プレイヤーがBustしてない場合、17を超えるまでカードを引く
      while (!playerBust && calc(this.hand) < 17) {
        //this.hand.push(pick());
      }
      this.$emit("result", calc(this.hand));
    },
    beforeMove: function(evt) {
      console.log(evt.draggedContext.element.name);
      return evt.draggedContext.element.name !== "3";
    },
    onEnd: function(evt) {
      //console.log(evt);
    },
    getSpaceLength(num) {
      if ((num = 2)) {
      }
    }
  }
};
</script>
