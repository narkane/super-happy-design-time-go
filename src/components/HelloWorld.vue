<template>
  <div id="data">
    <v-form ref="form">
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
            <v-text-field v-model="inchesxSprite" label="X inches"></v-text-field>
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
        <v-layout row>
          <v-flex xs1>
            <v-text-field v-model="drawArea.x" label="X" @change="changeDraw" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.y" label="Y" @change="changeDraw" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.width" label="width" @change="changeDraw" required></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.height" label="height" @change="changeDraw" required></v-text-field>
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
            <v-text-field v-model="drawArea.x / ratio" label="X inches" disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.y / ratio" label="Y inches" disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.width / ratio" label="width in." disabled></v-text-field>
          </v-flex>
          <v-flex xs1>
            <v-text-field v-model="drawArea.height / ratio" label="height in." disabled></v-text-field>
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
        backgroundColor: 0x1099bb,
        transparent: 0
      }),
      // testTex: PIXI.utils.TextureCache["../assets/blob.png"],
      ratio: 32,
      testSprite: PIXI.Sprite.from(blob),
      drawArea: new PIXI.Rectangle(300, 150, 400, 700)
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
      that.app.renderer.view.style.left = that.drawArea.x + "px";
      that.app.renderer.view.style.top = that.drawArea.y + "px";
      that.app.renderer.resize(that.drawArea.width, that.drawArea.height);
      document.body.appendChild(that.app.view);

      // graphics.beginFill(transparent);

      // set the line style to have a width of 5 and set the color to red
      // that.drawArea.lineStyle(2, 0x000000);

      alert("SCREAM!!!");
      // draw a rectangle
      // that.drawArea.width = 300;
      // that.drawArea.height = 600;
      // that.drawArea.drawRect(0, 0, that.drawArea.width, that.drawArea.height);
      // that.drawArea.x = 300;
      // that.drawArea.y = 150;

      // that.app.stage.width = that.draw

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

      // that.app.stage.addChild(that.drawArea);
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
          this.x = newPosition.x;
          this.y = newPosition.y;
        }
      }
    },
    changeDraw: function() {
      console.log("CHANGE!");
      // that.app.renderer.screen
      // this.drawArea.width = value;
      this.app.renderer.view.style.left = this.drawArea.x + "px";
      this.app.renderer.view.style.top = this.drawArea.y + "px";
      this.app.renderer.resize(this.drawArea.width, this.drawArea.height);
    }
  },
  created() {
    this.init();
  },
  computed: {
    inchesxSprite: {
      get() {
        return this.testSprite.x / this.ratio;
      },
      set(value) {
        this.testSprite.x = value * this.ratio;
      }
    }
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
