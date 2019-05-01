<template>
<div class="dealer">
  <v-container fluid grid-list-xl>
    <v-layout row wrap justify-start>
      <v-flex xs3 ma-1 pa-0>
        <div class="list vList">
          <span>[Card area]</span>
          <draggable :list="unfinishedTasks" :move="beforeMove" :options="{group:'tasks'}" :animation=300>
            <span>　a</span>
            <div class="pa-0 ma-0" v-for="task in unfinishedTasks" :key="task.id">
              <v-card dark color="primary" class="pa-2 ma-2 card" v-if="task.name != 'Card drag area'">
                  {{task.name}}
              </v-card>
            </div>
          </draggable>
        </div>
      </v-flex>
      <v-flex xs3 ma-2 pa-0>
        <div class="list vList">
          <draggable :list="finishedTasks":move="beforeMove" @end="onEnd" :animation=300 :options="{group:'tasks'}">
            <div class="pa-0 ma-0" v-for="task in finishedTasks" :key="task.id" >
              <v-card dark color="primary" class="pa-2 ma-2 card" v-if="task.name != 'Card drag area'">
                  {{task.name}}
              </v-card>
              <span v-else>
                {{task.name}}
              </span> 
            </div>
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
    unfinishedTasks:[
      {id:10, name:"Card drag area"}, 
      {id:11, name:"Create a document"}, 
      {id:12, name:"Appointment"}
    ],
    finishedTasks:[
      {id:20, name:"Card drag area"},
      {id:21, name:"Appointment"}
    ]
    };
  },
  methods: {
    beforeMove: function(evt) {
      console.log(evt.draggedContext.element.name);
      return evt.draggedContext.element.name !== "Card drag area";
    },
    postexec(playerBust) {
      this.hand[0].hide = false;
      // プレイヤーがBustしてない場合、17を超えるまでカードを引く
      while (!playerBust && calc(this.hand) < 17) {
        //this.hand.push(pick());
      }
      this.$emit("result", calc(this.hand));
    },
  }
  /*** 
  created: function() {
    //this.hand.push(pick());
    this.hand[0].hide = true;
    this.$on("postexec", this.postexec);
  },***/
};
</script>
