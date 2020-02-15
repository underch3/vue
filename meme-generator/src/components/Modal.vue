<template>
  <div class="container row">
    <v-stage ref="printMe" :config="stageSize">
      <v-layer ref="layer">
        <v-image
          :config="{
            image: image,
            width: 500,
            height: 500
          }"
        />
        <v-text
          @dragstart="handleDragStart"
          @dragend="handleDragEnd"
          :config="{
            text: (text1.toUpperCase()),
            x: 10,
            y: 10,
            width: 480,
            height: 480,
            stroke:'black',
            fontFamily: 'Anton',
            fontSize: 40,
            fontStyle: 'bold',
            align: 'center',
            wrap: word,
            draggable: true,
            fill: colorCode,
          }"
        />
        <v-text
          @dragstart="handleDragStart"
          @dragend="handleDragEnd"
          :config="{
            text: (text2.toUpperCase()),
            x: 10,
            y: 450,
            width: 480,
            height: 480,
            stroke:'black',
            fontFamily: 'Anton',
            fontSize: 40,
            fontStyle: 'bold',
            align: 'center',
            wrap: word,
            draggable: true,
            fill: colorCode,
          }"
        />
      </v-layer>
    </v-stage>

    <div class="editForm col">
      <b-form>
        <b-form-group id="input-group-1" label="Top Text" label-for="input-1">
          <b-form-input
            class='inputs'
            id="input-1"
            v-model="text1"
            placeholder="Top Text"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-2"
          label="Bottom Text"
          label-for="input-2"
        >
          <b-form-input
            class='inputs'
            id="input-2"
            v-model="text2"
            placeholder="Bottom Text"
          ></b-form-input>
        </b-form-group>
        <b-row class="flex">
        <label class="label" :for="`type-${type}`">Pick text color:</label>
        <b-form-input :id="`type-${type}`" :type="type" v-model="colorCode" class="color-picker" label="Top Text"></b-form-input>
        </b-row>
        <b-form-group class="save">Save result by mouse right-click on meme and 'Save image as..'</b-form-group>
        <b-button @click="print">Hello</b-button>
      </b-form>
    </div>
  </div>
</template>
<script>
var width = 500;
var height = 500;
export default {
  name: "Modal",
  props: {
    imageInModal: {}
  },
  data() {
    return {
      text1: "",
      text2: "",
      type: 'color',
      colorCode: '#ffffff',
      stageSize: {
        width: width,
        height: height
      },
      image: null,
      output: null,
      isDragging: false
    };
  },
  created() {
    const image = new window.Image();
    image.src = this.imageInModal;
    image.onload = () => {
      this.image = image;
    };
  },

  methods: {
    handleDragStart() {
      this.isDragging = true;
    },
    handleDragEnd() {
      this.isDragging = false;
    },
    print() {
      const el = this.$refs.printMe;
      // add option type to get the image version
      // if not provided the promise will return 
      // the canvas.
      const options = {
        type: 'dataURL'
      }
      this.output = await this.$html2canvas(el, options);
    }
  }
};
</script>

<style scoped>
.color-picker
{
  width:20%;
}
.flex{
  display: flex;
  align-items: center;
  margin-left:3px;
}
.save{
  margin-top:10px;
  margin-left: 3px;
  font-weight: bold;
}
</style>
