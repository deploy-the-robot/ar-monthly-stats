<template>

  <li class="ar-stats-list__row">

    <!-- <div class="ar-stats-list__icon" :class="[isIncrease ? 'color-green' : 'color-red']"> -->
    <div class="ar-stats-list__icon">
      <i class="fa">
        <span contenteditable="true" v-html="icon" @input="updateIcon" @click="selectText" :class="[isIncrease ? 'color-green' : 'color-red']"></span>
        <i class="fa fa-arrow-up" v-if="isIncrease"></i>
        <i class="fa fa-arrow-down" v-else></i>      
      </i>
    </div>

    <div class="ar-stats-list__col text-left">
      <span contenteditable="true">{{ title }}</span>
      <div class="progress-bar-group" style="width: 100%">
        <!-- <span>&#8722</span> -->
        <div class="progress-bar progress-bar--reversed">
          <div class="bar" :data-percent="percentDecrease">
            
          </div>
        </div>
        <div class="progress-bar__separator"></div>
        <div class="progress-bar">
          <div class="bar" :data-percent="percentIncrease">
          
          </div>
        </div>
        <!-- <span>+</span> -->
      </div>

    </div>
    <div class="ar-stats-list__col">
      
      <span class="ar-stats-list__num" 
      contenteditable="true"
      @input="updateMonth_1"
      >{{ monthData_1 }}</span>
      <span class="ar-stats-list__stat-label">{{ monthLabel_1 }}</span>
    </div>
    <div class="ar-stats-list__col">
      
      <span class="ar-stats-list__num" 
      contenteditable="true"
      @input="updateMonth_2"
      >{{ monthData_2 }}</span>
      <span class="ar-stats-list__stat-label">{{ monthLabel_2 }}</span>
    </div>



  </li>

</template>

<script>


export default {
    name: 'App',
    props: {
      title: {
        type: String,
        default: 'Enter title'
      },
      monthLabel_1: {
        type: String,
        required: true
      },
      monthLabel_2: {
        type: String,
        required: true
      },
      monthData_1: {
        type: String,
        default: '100'
      },
      monthData_2: {
        type: String,
        default: '120'
      },
      icon: {
        type: String,
        default: '&#xf508'
      }
    },
    data() {
      return {
        // monthData_1: "123",
        // monthData_2: "156",
        // icon: "&#xf508"
      }
    },
    computed: {
      monthNumber_1() {
        return parseInt(this.stripToNumeric(this.monthData_1))
      },
      monthNumber_2() {
        return parseInt(this.stripToNumeric(this.monthData_2))
      },
      isIncrease() {
        const n1 = this.monthNumber_1
        const n2 = this.monthNumber_2
        return n2 > n1 ? true : false
      },
      percentIncrease() {
        const n1 = this.monthNumber_1
        const n2 = this.monthNumber_2
        if (n2 > n1) {
          const increase = n2 - n1
          return Math.round(increase / n1 * 100)
        } else return 0
      },

      percentDecrease() {
        const n1 = this.monthNumber_1
        const n2 = this.monthNumber_2
        if (n1 > n2) {
          const decrease = n1 - n2
          return Math.round(decrease / n1 * 100)
        } else return 0
      }
    },
    methods: {
      stripToNumeric(str) {
        return str.replace(/\D/g,'');
      },
      updateMonth_1(e) {
        this.$emit('update:monthData_1', e.target.innerText)
      },
      updateMonth_2(e) {
        this.$emit('update:monthData_2', e.target.innerText)
      },
      selectText(e) {
        var cell = e.target;
        console.log(cell)
        var range, selection;
        if (document.body.createTextRange) {
          range = document.body.createTextRange();
          range.moveToElementText(cell);
          range.select();
        } else if (window.getSelection) {
          selection = window.getSelection();
          range = document.createRange();
          range.selectNodeContents(cell);
          selection.removeAllRanges();
          selection.addRange(range);
        }
      },
      updateIcon(e) {
        if (
          e.target.innerText == "&#x" || 
          e.target.innerText == "" ||
          e.target.innerText.length !== 4 ) {
          this.$emit('update:icon', '?')
        } else {
          this.$emit('update:icon', "&#x" + e.target.innerText)
        }
      }
    }
  }


</script>