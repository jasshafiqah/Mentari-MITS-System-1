<template>
<div class="box" style="position: relative;">
 <i :style="colorTextStyle" class="fa fa-bell"></i><div style="float:right;">
 <span 
 class="badge bg-danger" 
 style="color:white; 
 border-radius: 50%; 
 position: absolute;
 top: -10px;
right: -10px;
  ">12</span>
 </div>
</div>
  <!-- <svg
    class="va-icon-notification"
    xmlns="http://www.w3.org/2000/svg"
    width="20"
    height="20"
    viewBox="0 0 20 20"
  >
    <g fill="none" fill-rule="evenodd">
      <path d="M-2-2h24v24H-2z"/>
      <path
        :fill="color"
        fill-rule="nonzero"
        d="M10 20c1.1 0 2-.9 2-2H8c0 1.1.9 2 2 2zm6-6V9c0-3.07-1.63-5.64-4.5-6.32V2c0-.83-.67-1.5-1.5-1.5S8.5 1.17 8.5 2v.68C5.64 3.36 4 5.92 4 9v5l-2 2v1h16v-1l-2-2zm-2 1H6V9c0-2.48 1.51-4.5 4-4.5s4 2.02 4 4.5v6zM5.58 2.08L4.15.65C1.75 2.48.17 5.3.03 8.5h2a8.445 8.445 0 0 1 3.55-6.42zM17.97 8.5h2c-.15-3.2-1.73-6.02-4.12-7.85l-1.42 1.43a8.495 8.495 0 0 1 3.54 6.42z"
      />
    </g>
  </svg> -->

</template>

<script>
export default {
  name: 'VaIconMessage',

  data () {
    return {
      color: "white",
      colorTextStyle: "color: "+this.$store.getters.paletteText
    }
  },

  watch: {
    '$store.getters.paletteText': function() {
      this.colorTextStyle = 'color: '+this.$store.getters.paletteText;
    }
  },
  // props: {
  //   color: {
  //     type: String,
  //     default: 'white',
  //   },
  // },
}
</script>

<style lang="scss">
  .va-icon-notification {
    display: inline-block;
    width: 24px;
    height: 24px;
  }
</style>
