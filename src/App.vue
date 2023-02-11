<template>
  <div class="ar-edm-mock">
    <div class="ar-edm-mock__content">

      <div class="ar-edm-mock__section">
        <h1>Monthly Snapshot</h1>
        <h2>Instructions</h2>
        <p>
          <b>Headings:</b> Click headings to change <br><br>
          <b>Data:</b> Click number to change (percentage values are auto-calculated)<br><br>
          <b>Dates:</b> Enter date in 2 fields below <br><br>
        </p>
        <div class="ar-flex ar-flex--justify-around">
          <div>
            <label for="month1">Month 1 label:</label>
            <input type="text" id="month1" name="month1" v-model="monthLabel_1">
          </div>
          <div>
            <label for="month2">Month 2 label:</label>
            <input type="text" id="month2" name="month2" v-model="monthLabel_2">
          </div>
        </div>

      </div>

      <div class="ar-edm-mock__section">
        <div>
        
          <div class="ar-edm-mock__tile bg-white"> 
            <img src="src/assets/header.png" alt="" class="ar-edm-header">
            <div class="ar-edm-mock__tile-content bg-white" id="js-html-to-png-padded">
              <ul class="ar-stats-list bg-white" id="js-html-to-png">
              <ArStatsListItem
                v-for="stat in stats"

                :icon="stat.icon"
                :title="stat.title"
                :monthData_1="stat.monthData_1"
                :monthData_2="stat.monthData_2"

                v-on:update:monthData_1="stat.monthData_1 = $event"
                v-on:update:monthData_2="stat.monthData_2 = $event"
                v-on:update:icon="stat.icon = $event"
                v-on:update:title="stat.title = $event"
                
                :monthLabel_1="monthLabel_1"
                :monthLabel_2="monthLabel_2">
              </ArStatsListItem>
            </ul>
            </div>
          </div>
        </div>
      </div>

      <div class="ar-edm-mock__section" >
        <div class="button" @click="downloadImg('js-html-to-png-padded')">Download (padding)</div>
        <div class="button" @click="downloadImg('js-html-to-png')">Download (no padding)</div>
        
      </div>
      <br>


    </div>
  </div>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>




<script>
import ArStatsListItem from './components/ar-stats-list-item.vue'
import * as htmlToImage from 'html-to-image'
import FileSaver from 'file-saver'

export default {
    name: 'App',
    components: {
      ArStatsListItem
    },
    data() {
      return {
        monthLabel_1: 'Dec 2022',
        monthLabel_2: 'Jan 2023',
        stats: [
          {
            icon: "&#xf508",
            title: "Number of new advisers per month",
            monthData_1: "101",
            monthData_2: "156",
          },
          {
            icon: "&#xf3fd",
            title: "Adviser average AQS Score",
            monthData_1: "954",
            monthData_2: "1,002",
          },
          {
            icon: "&#xf1ad",
            title: "Number of new practices per month",
            monthData_1: "12",
            monthData_2: "10",
          },
          {
            icon: "&#xf2e8",
            title: "Average Client Lead FUA",
            monthData_1: "$18,469",
            monthData_2: "$19,659",
          },
          {
            icon: "&#xf1da",
            title: "Average Client Lead Age",
            monthData_1: "55",
            monthData_2: "51",
          }
        ]
      }
    },
    methods: {
      downloadImg(id) {
        var node = document.getElementById(id);
        htmlToImage.toBlob(node)
        .then(function (blob) {
            if (window.saveAs) {
              window.saveAs(blob, 'monthly-roundup.png');
            } else {
            FileSaver.saveAs(blob, 'monthly-roundup.png');
          }
        });
      },
    },
    computed: {},
    mounted() {
      
    }
  }




</script>

