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
  data: () => {
    return {
      type: "WebGL",
      app: new PIXI.Application({
        width: 800,
        height: 600,
        backgroundColor: 0x1099bb
      }),
      // testTex: PIXI.utils.TextureCache["../assets/blob.png"],
      ratio: 15,
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
      that.testSprite.x = 400;
      that.testSprite.y = 300;
      // that.testSprite.position.set(
      //   that.app.screen.width / 2,
      //   that.app.screen.height / 2
      // );
      that.testSprite.interactive = true;
      that.testSprite.buttonMode = true;

      that.testSprite.on("pointerdown", function() {
        that.testSprite.scale.x *= 1.25;
        that.testSprite.scale.y *= 1.25;
      });

      that.app.stage.addChild(that.testSprite);

      that.app.ticker.add(function(delta) {
        that.testSprite.rotation += 0.1 * delta;
      });
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
