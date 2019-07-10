<template>
  <div id="data">
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-container fluid>
        <v-layout row>
          <v-flex xs1>
            <v-text-field v-model="testSprite.x" label="X" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.y" label="Y" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.width" label="width" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.height" label="height" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field disabled />
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="ratio" label="px/inch" required />
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs1>
            <v-text-field v-model="testSprite.x / ratio" label="X inches" disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.y / ratio" label="Y inches" disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.width / ratio" label="width in." disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="testSprite.height / ratio" label="height in." disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field disabled />
          </v-flex>
          <v-flex xs1>
            <v-text-field disabled />
          </v-flex>
        </v-layout>
      </v-container>
      <v-btn color="green">Save</v-btn>
      <v-btn color="orange">Reset</v-btn>
      <v-btn color="red">Cancel</v-btn>
    </v-form>
  </div>
</template>

<script>
import * as PIXI from "pixi.js";
import blob from "../assets/blob.png";

export default {
  name: "HelloWorld",
  data() {
    return {
      type: "WebGL",
      app: new PIXI.Application({
        width: 600,
        height: 800,
        backgroundColor: 0x1099bb
      }),
      // testTex: PIXI.utils.TextureCache["../assets/blob.png"],
      ratio: 32,
      testSprite: PIXI.Sprite.from(blob),
      height: 0,
      width: 0,
      x: 0,
      y: 0,
      valid: null
    };
  },
  props: {
    msg: String
  },
  methods: {
    init: function() {
      var that = this;

      if (!PIXI.utils.isWebGLSupported()) {
        that.type = "canvas";
      }

      document.body.appendChild(that.app.view);

      // setup sprites
      // var texture = PIXI.Texture.fromImage("../assets/blob.png");
      // that.testSprite = PIXI.Sprite.from(blob);
      that.testSprite.anchor.set(0.5);
      that.testSprite.x = that.app.screen.width / 2;
      that.testSprite.y = that.app.screen.height / 2;
      // that.testSprite.position.set(
      //   that.app.screen.width / 2,
      //   that.app.screen.height / 2
      // );
      that.testSprite.interactive = true;
      that.testSprite.buttonMode = true;

      that.testSprite
        .on("mousedown", onDragStart)
        .on("touchstart", onDragStart)
        // events for drag end
        .on("mouseup", onDragEnd)
        .on("mouseupoutside", onDragEnd)
        .on("touchend", onDragEnd)
        .on("touchendoutside", onDragEnd)
        // events for drag move
        .on("mousemove", onDragMove)
        .on("touchmove", onDragMove);

      // that.testSprite.scale.y *= 1.25;

      that.app.stage.addChild(that.testSprite);

      that.app.ticker.add(function(delta) {
        that.testSprite.rotation += 0.1 * delta;
      });

      function onDragStart(event) {
        // store a reference to the data
        // the reason for this is because of multitouch
        // we want to track the movement of this particular touch
        this.data = event.data;
        this.alpha = 0.5;
        this.dragging = true;
      }

      function onDragEnd() {
        this.alpha = 1;

        this.dragging = false;

        // set the interaction data to null
        this.data = null;
      }

      function onDragMove() {
        if (this.dragging) {
          var newPosition = this.data.getLocalPosition(this.parent);
          this.position.x = newPosition.x;
          this.position.y = newPosition.y;
        }
      }
    }
  },
  mounted() {
    this.init();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#data {
  position: absolute;
  top: 350px;
  right: -100px;
  width: 800px;
  height: 200px;
}
canvas {
  position: absolute;
  top: 0px;
  left: 0px;
  z-index: -10;
}
input {
  text-align: center;
}
</style>
