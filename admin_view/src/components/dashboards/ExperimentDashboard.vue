<template>
  <div class="stage">
    <main class="main-content">
      <div class="container-fluid">
        <h1 class="h4 mb-4">Dashboard</h1>
      </div>
      <div class="container-fluid">
        <div class="row">
          <DashboardItem
            v-for="item in applications" v-bind:dashboard_item="item" v-bind:key="item.title" v-bind:height="height">
          </DashboardItem>
        </div>
      </div>
    </main>
    <aside class="sidebar">
      <header class="sidebar-header">
        <h1 class="sidebar-header__title">Recent Experiments</h1>
        <a href="#0" class="sidebar-header__action" v-on:click="views_all_click_handler">View all</a>
      </header>

      <ol class="feed">
        <RecentExperiment v-for="experiment in experiments"
                          v-if="experiment.index <default_experiment_count || view_all" v-bind:experiment="experiment"
                          v-bind:key="experiment.name"></RecentExperiment>
      </ol>
    </aside>
  </div>
</template>
<script>
  import DashboardItem from '../DashboardItem.vue'
  import RecentExperiment from '../RecentExperiment.vue'
  import {addIndex} from '../../utils.js'

  export default{
    'name':'main-section',
    components: {
      DashboardItem, RecentExperiment
    },
    data:function () {

      var data_returned={
        "view_all": false,
        "default_experiment_count": 3,
        "height":180,
        "applications":
          [

          ]
        , "experiments": [
          {
            "name": "Gaussian",
            "description": "My very first test experiment",
            "status": "Failed",
            "updated": "14 minutes ago"
          },
          {
            "name": "Lampps",
            "description": "A really BIG experiment That Has a Really Long Title",
            "status": "Completed",
            "updated": "20 hours ago"
          },
          {
            "name": "Gromacs",
            "description": "exp_4a56w4892s23r6p9y_1",
            "status": "Created",
            "updated": "2 days ago"
          },
          {
            "name": "RandExpr",
            "description": "exp_4a56w4892s23r6p9y_1",
            "status": "Failed",
            "updated": "5 days ago"
          }
        ]
      };
      addIndex(data_returned["experiments"]);
      return data_returned;
    },
    mounted:function () {
      this.fetchApplications();
    },
    methods: {
      "views_all_click_handler": function () {
        this.view_all = !this.view_all;
      },
      fetchApplications:function () {
        var convert=function (applications) {

        };
        this.$http.get('/api/applications').then(response => {
          this.applications=response.body;
        }, response => {
          this.applications=[{
            "appModuleId": "",
            "appModuleName": "No Applications Found",
            "appModuleDescription": "",
            "appModuleVersion": ""
          }]
        });
      }

    }
  }
</script>
