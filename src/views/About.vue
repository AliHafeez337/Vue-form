<template>
  <div class="about">
    <div class="container">
      <div class="row">
        <div class="col-lg-3">
        </div>
        <div class="col-lg-6">
          <form>
            <div class="form-group">
              <label for="firstname">First Name</label>
              <br />
              <span class="text-danger text-sm" v-show="this.firstname === '' && this.firstnameField > 0">First name is required.</span>
              <input
                v-model="firstname"
                v-on:click="firstnameField += 1"
                v-on:keyup.tab="firstnameField += 1"
                type="text"
                name="firstname"
                class="form-control">
            </div>
            <div class="form-group">
              <label for="lastname">Last Name</label>
              <br />
              <span class="text-danger text-sm" v-show="this.lastname === '' && this.lastnameField > 0">Last name is required.</span>
              <input 
                v-model="lastname"
                v-on:click="lastnameField += 1"
                v-on:keyup.tab="lastnameField += 1"
                type="text"
                name="lastname"
                class="form-control">
            </div>

            <div class="form-group">
              <label for="email">Email</label>
              <br />
              <span class="text-danger text-sm" v-show="this.email === '' && this.emailField > 0">Email is required.</span>
              <span class="text-danger text-sm" v-show="isEmailValid()">Email must be valid.</span>
              <input
                v-on:click="emailField += 1"
                v-on:keyup.tab="emailField += 1"
                type="text"
                class="form-control"
                name="email"
                v-model="email"/>
            </div>
            
            <div class="form-group">
              <label for="countries">Country</label>
              <br />
              <span class="text-danger text-sm" v-show="this.country === '' && this.countryField > 0">Counrtry is required.</span>
              <br />
              <select
                v-on:click="countryField += 1"
                v-on:keyup.tab="countryField += 1"
                class="btn btn-primary" 
                name="countries" 
                @change="selectCountry(select1)" 
                v-model="select1">
                <option 
                  v-for="(item, index) in countryOptions" 
                  :key="index" 
                  :value="item.label"
                  >{{item.label}}</option>
              </select>            
            </div>

            <div class="form-group">
              <label for="cities">City</label>
              <br />
              <span class="text-danger text-sm" v-show="this.city === '' && this.cityField > 0">City is required.</span>
              <br />
              <select
                v-on:click="cityField += 1"
                v-on:keyup.tab="cityField += 1"
                class="btn btn-primary" 
                name="cities" 
                @change="selectCity(select2)" 
                v-model="select2">
                <option 
                  v-for="(item, index) in cityOptions" 
                  :key="index" 
                  :value="item"
                  >{{item}}</option>
              </select>            
            </div>

            <div class="form-group">
              <label for="name">Fruits</label>
              <br />
              <select
                class="btn btn-primary" 
                name="fruits" 
                v-model="select3">
                <option 
                  v-for="(item, index) in fruits" 
                  :key="index" 
                  :value="item"
                  >{{item}}</option>
              </select>            
            </div>

            <div class="form-group">
              <label for="fileToUpload">Select file (not working, just a button)
              </label>
              <br />
              <input
                style="display: none"
                type="File" 
                name="fileToUpload" 
                id="fileToUpload"
                ref="fileInp">
              <button
                type="button"
                class="btn btn-primary"
                @click="$refs.fileInp.click()" 
              >Select file</button>
            </div>

            <div class="form-group">
              <p v-show="this.isHuman">Now, you are a human...</p>
              <VueRecaptcha sitekey="6Ld9kOwUAAAAAPC7LuvcDJp5u5DYlGHCfOV5ldog" :loadRecaptchaScript="true" @verify="valid"/>
            </div>
            <input 
              :disabled="isDisabled"
              @click.prevent="submit()"
              type="submit" 
              value="Submit" 
              class="btn btn-primary">
          </form>
          <br />
          <br />
        </div>
        <div class="col-lg-3">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueRecaptcha from 'vue-recaptcha'
import axios from 'axios'
import request from 'request'

var jsonData = require('./countries_json.json');
var jsonData1 = require('./world-cities_json.json');

export default {
  components: {VueRecaptcha},
  data() {
    return {
      sitekey: '6Ld9kOwUAAAAAPC7LuvcDJp5u5DYlGHCfOV5ldog',
      isHuman: false,
      lastname: '',
      lastnameField: '',
      firstname: '',
      firstnameField: '',
      select1: '',
      country: '',
      countryOptions: jsonData,
      countryField: 0,
      select2: '',
      city: '',
      cityField: 0,
      cityOptions: [],
      select3: '',
      fruits: ['apple', 'mango', 'banana', 'grapes', 'strawbery', 'dates'],
      email: '',
      emailField: 0,
      reg: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/
    }
  },
  computed: {
    isDisabled () {
      return (!this.isHuman || this.firstname === '' || this.lastname === '' || this.isEmailValid === '' || !this.reg.test(this.email) || this.country === '' || this.city == '')
    },
  },
  methods: {
    selectCountry(count) {
      this.country = count
    },
    selectCity(count){
      this.city = count
    },
    valid (response) {
      this.isHuman = true;
    },
    isEmailValid () {
      return !((this.email === '') ? true : (this.reg.test(this.email)))
    },
    submit() {
      const { isHuman, firstname, lastname, email, country, city } = this
      if (!this.isDisabled){
        var obj = {
          firstname, lastname, email, country, city
        }
        console.log(obj)
      }
    }
  },
  watch: {
    country: function() {
      if (this.country !== null || this.country !== undefined){
        for (var key in jsonData1) {
          if (key == this.country){
            const uniqueSet = new Set(jsonData1[key])
            this.cityOptions = [...uniqueSet]
            break
          }
        }
      }
    }
  }
}
</script>