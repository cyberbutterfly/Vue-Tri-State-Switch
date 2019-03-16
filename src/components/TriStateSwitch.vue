<template>
  <div class="wrapper" :style="changeBackground">
    <!-- No Choice -->
    <label for="no_radio" id="no-lbl" class="no-lbl" @click="selectedNo"></label>
    <input type="radio" name="choice_radio" id="no_radio" class="no_radio" :checked="checkedNo">
    <!-- Maybe Choice -->
    <label for="maybe_radio" id="maybe-lbl" class="maybe-lbl" @click="selectedMaybe"></label>
    <input type="radio" name="choice_radio" id="maybe_radio" class="maybe_radio" :checked="checkedMaybe">
    <!-- Yes Choice -->
    <label for="yes_radio" id="yes-lbl" class="yes-lbl" @click="selectedYes"></label>
    <input type="radio" name="choice_radio" id="yes_radio" class="yes_radio" :checked="checkedYes">
    <!-- Toggle -->
    <div class="toggle"></div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      toggleValue: 0 // Non-selected as default,
    }
  },
  methods: {
    selectedNo () {
      this.toggleValue = 0
    },
    selectedMaybe () {
      this.toggleValue = 1
    },
    selectedYes () {
      this.toggleValue = 2
    }
  },
  computed: {
    checkedNo () {
      if (this.toggleValue === 0) {
        return 'checked'
      } else {
        return undefined
      }
    },
    checkedMaybe () {
      if (this.toggleValue === 1) {
        return 'checked'
      } else {
        return undefined
      }
    },
    checkedYes () {
      if (this.toggleValue === 2) {
        return 'checked'
      } else {
        return undefined
      }
    },
    changeBackground () {
      if (this.toggleValue === 0) {
        return 'background: #FF3739'
      } else if (this.toggleValue === 1) {
        return 'background: #d8d8d8'
      } else {
        return 'background: #00D014'
      }
    }
  }
}
</script>

<style lang="scss" scoped>
/* height */
$h: 4rem;
/* width */
$w: $h * 2.2;
/* toggle dimensions */
$tw: $h * 0.9;
/* font size */
$fs: 2rem;
/* toggle's border thickness */
$bt: 0.1rem;
/* toggle box shadow */
$bs: $bt * 2;
/* labels animation time */
$ta: 6s;
/* label text color */
$text-color: white;

/* colors: si, ?, no*/
$colors: #FF3739, #ebebeb, #00D014;

@mixin borderColor($i) {
  border: $bt solid white;
  background: lighten(white, 10%);
  // box-shadow: 0 0 $bs $bs/2 nth($colors, $i) inset,
  //             0 0 $bs $bs/2 nth($colors, $i);
}

@mixin animationDelay($time) {
  -webkit-animation-delay: $time;
  -o-animation-delay: $time;
  -moz-animation-delay: $time;
  animation-delay: $time;
}

@mixin animation($time) {
  -webkit-animation: $time rot-label ease-in-out infinite;
  -moz-animation: $time rot-label ease-in-out infinite;
  -o-animation: $time rot-label ease-in-out infinite;
  animation: $time rot-label ease-in-out infinite;
}

.wrapper {
  width: $w;
  height: $h;
  position: relative;
  margin: 2rem auto;
  border-radius: $h/2;
  background: #d8d8d8;
  
  & .toggle {
    width: $tw;
    height: $tw;
    position: absolute;
    left: $w / 3 + ($tw - $h) / 2;
    top: -($tw - $h) / 2;
    border-radius: 50%;
    box-sizing: border-box;
    @include borderColor(2);    
    transition: all .3s cubic-bezier(0.175, 0.885, 0.320, 1.275);
  }

  & label {
    cursor: pointer;
    width: $h;
    height: $h;
    position: absolute;
    margin: 0;
    padding: 0;
    z-index: 1;
    display: inline-block;  
    
    text-align: center;
    line-height: $h;
    text-transform: uppercase;
    font-family: 'Lato', sans-serif;
    font-size: $fs;
    font-weight: bold;
    color: $text-color;
    @include animation($ta);
  }
  
  & input {
    position: absolute;
    left: 0;
    margin: 0;
    padding: 0;
    opacity: 0;
  }
}

.no_radio:checked {
  ~ .toggle {
    @include borderColor(1);
    left: -($tw - $h) / 2;
  }
}

.yes_radio:checked {
  ~ .toggle {
    @include borderColor(3);
    left: $w * 2 / 3 + ($tw - $h) * 2;
  }
}

.no_radio, .no-lbl {
  left: 0%;
}

.maybe_radio, .maybe-lbl {
  left: 100% * 1 / 3;
}

.yes_radio, .yes-lbl {
  left: 100% * 2 / 3;
}

.maybe-lbl {
  @include animationDelay($ta / 3);
}

.yes-lbl {
  @include animationDelay($ta * 2 / 3);
}
</style>
