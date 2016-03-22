<template>
  <div class="dashboard-header">
    <div class="menu-btn" v-on:click="toggleMenu()" v-el:menu-btn>
      <i v-bind:class="['fa', 'fa-fw', $parent.menuOpen ? 'fa-times' : 'fa-bars']"></i>
    </div>

    <h2>Organize Instagram</h2>
  </div>

  <div class="add-feed-container">
    <section class="add-instagram-section">
      <h2>Instagram API Client ID</h2>
      <p>Enter your Instagram app API Client ID</p>
      <div class="add-insta-input">
        <input type="url" class="form-control" v-model="instagramid" v-on:keyup.enter="saveSettings()" placeholder="Put ID string here">
      </div>
      <br/>
      <p>Enter your Instagram API Client Secret</p>
      <div class="add-insta-input">
        <input type="url" class="form-control" v-model="instagramsecret" v-on:keyup.enter="saveSettings()" placeholder="Put Secret string here">
      </div>
      <div v-if="alert" class="alert danger">
        <p>{{ alertmessage }}</p>
      </div>
      <p><button v-bind:disabled="processed" v-on:click="saveSettings()" type="button" class="add-button">Save Settings</button></p>
    </section>
    <br/>
  </div>
</template>
<script>
import store from '../store.js'
import service from '../helpers/services'
import queue from '../helpers/queue'
import _ from 'lodash'
import online from 'is-online'
import async from 'async'
var app = require('remote').require('app')
var jetpack = require('fs-jetpack')
var useDataDir = jetpack.cwd(app.getPath("userData") + '/streams/')
var Ps = require('perfect-scrollbar');
var dialog = require('remote').require('dialog')
var services = require('../helpers/services.js')

const {
  saveSettings,
} = store.actions

export default{
  data(){
    return {
      instagramsecret: '',
      instagramid: ''
    }
  },
  computed: {
  },
  ready(){
    console.log('ready()... store.state', store.state);

    service.fetchSettings().then((settings) => {
      console.log('ready() fetchSettings settings:', settings);
      if (settings) {
        this.instagramid = settings.instagramId;
        this.instagramsecret = settings.instagramSecret;
      }
    })
  },
  methods: {
    saveSettings(){
      console.log('saveSettings()... this.instagramid:', this.instagramid, 'this.instagramsecret:', this.instagramsecret);

      // Save the key to the database.
      service.addSettings({instagramId: this.instagramid, instagramSecret: this.instagramsecret}, function(settings) {
        console.log('settings:', settings);
      });
    },
    toggleMenu() {
      this.$parent.toggleMenu();
    }
  }
}
</script>
