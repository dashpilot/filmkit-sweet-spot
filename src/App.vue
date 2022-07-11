
<template>
<div class="wrap">
  <main id="main">

    <div class="row g-0">
      <div class="col-md-8">



        <div class="wdgt wdgt-main">
          <div class="wdgt-toolbar">
            Filmkit Aspect Ratio Calculator
          </div>



          <div class="ratio">{{ratio}}</div>

        </div>

      </div>
      <div class="col-md-4">

        <div class="wdgt wdgt-side">
          <div class="wdgt-toolbar">Controls</div>

          <div class="wdgt-body">

            <label>Original Width</label>
            <input type="number" class="form-control" v-model="orig_w" @keyup="calculate($event, 'w')">

            <label>Original Height</label>
            <input type="number" class="form-control" v-model="orig_h" @keyup="calculate($event, 'h')">

            <label>New Width</label>
            <input type="number" class="form-control" v-model="new_w" @keyup="calculate($event, 'w')">

            <label>New Height</label>
            <input type="number" class="form-control" v-model="new_h" @keyup="calculate($event, 'h')">

          </div>

          <div class="wdgt-footer">


          </div>

        </div>


      </div>

      <div class="text-center" id="footer">Made by <a href="https://www.filmkit.net">Filmkit</a></div>

    </div>




  </main>
</div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      orig_w: 1920,
      orig_h: 1080,
      new_w: null,
      new_h: null,
      ratio: "16:9"
    }
  },
  methods: {
    calculate($event, trigger) {

      let keyCode = ($event.keyCode ? $event.keyCode : $event.which);
      if (keyCode < 48 || keyCode > 57) { // 46 is dot
        $event.preventDefault();
      }

      let ratio = this.orig_h / this.orig_w;
      console.log(ratio)
      console.log(this.new_w)

      if (trigger == "w") {
        this.new_h = Math.round(this.new_w * ratio);
      } else {
        this.new_w = Math.round(this.new_h * ratio);
      }

      this.setRatio($event);

    },
    setRatio($event) {

      let keyCode = ($event.keyCode ? $event.keyCode : $event.which);
      if (keyCode < 48 || keyCode > 57) { // 46 is dot
        $event.preventDefault();
      }

      let ratio = this.orig_h / this.orig_w;

      if (ratio == 0.5625 || ratio == 1.7777777777777777) {
        this.ratio = "16:9";
      } else if (ratio == 1) {
        this.ratio = "1:1";
      } else if (ratio == 0.75 || ratio == 1.3333333333333333) {
        this.ratio = "4:3";
      } else if (ratio == 0.6666666666666666 || ratio == 1.5) {
        this.ratio = "3:2";
      } else if (ratio == 0.625 || ratio == 1.6) {
        this.ratio = "16:10";
      } else if (ratio == 0.5405405405405405 || ratio == 1.85) {
        this.ratio = "1.85:1";
      } else if (ratio == 0.41841004184100417 || ratio == 2.39) {
        this.ratio = "2.39:1";
      } else {
        if (Math.round(ratio) == ratio) {
          this.ratio = ratio + ":1";
        } else {
          this.ratio = ratio.toFixed(2) + ":1";
        }
      }

    }
  }
}
</script>

<style>
.ratio {
  width: 150px;
  height: 150px;
  border: 3px solid black;
  border-radius: 50%;
  text-align: center;
  font-size: 40px;
  padding-top: 42px;
  margin: 15% auto;
}
</style>
