<template>
  <div class="dashboard-header">
    <div class="menu-btn" v-on:click="toggleMenu()" v-el:menu-btn>
      <i v-bind:class="['fa', 'fa-fw', $parent.menuOpen ? 'fa-times' : 'fa-bars']"></i>
    </div>

    <h2>Organize Instagram</h2>
  </div>

  <div class="add-feed-container">
    <section class="add-key-section">
      <h2>Instagram API Key</h2>
      <p>Enter your Instagram app API key.</p>
      <div class="add-key-input">
        <input type="url" class="form-control" v-model="instagramkey" v-on:keyup.enter="saveKey()" placeholder="Put key string here">
      </div>
      <div v-if="alert" class="alert danger">
        <p>{{ alertmessage }}</p>
      </div>
      <p><button v-bind:disabled="processed" v-on:click="saveKey()" type="button" class="add-button">Save Key</button></p>
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
  saveKey,
} = store.actions

export default{
  data(){
    return {
      instagramkey: '',
    }
  },
  computed: {
  },
  ready(){
    console.log('ready()... store.state', store.state);

    // this.instagramkey = _.where(store.state.instagramKey, { 'instagramKey': this.instagramkey });
    service.fetchInstagramKey().then((key) => {
      console.log('key:', key);
      this.instagramkey = key;
    })
  },
  methods: {
    saveKey(){
      console.log('saveKey()... this.instagramkey:', this.instagramkey);

      // Save the key to the database.
      service.addInstagramKey(this.instagramkey, function(keySettings) {
        console.log('keySettings:', keySettings);
      });
    },
    toggleMenu() {
      this.$parent.toggleMenu();
    }
  }
}
</script>
