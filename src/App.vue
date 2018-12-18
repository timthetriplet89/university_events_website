<template>

  <div id="app">

    <vue-headful
            title="Campus Events"
            description="mobile events website" />

    <h1>BYU-Idaho Events</h1>

    <div class="row justify-content-center" >
    <div>

      <form class="margin-top " @submit.prevent="filterBySearch" v-show="showSearchBar()">
        <!-- width-auto -->

        <div class="row">
          <!-- justify-content-center -->
        <input class="form-control-custom inline " placeholder="Enter Keyword" v-model="search.keyword"/>
        <button class="btn success inline" type="submit">Search</button>  
        </div>
        <!-- <b-button :size='sm' :variant='primary' type="submit">Search</b-button>   -->
        <!-- @click="filterBySearch()" -->
        <!-- border-input -->
      </form>

    </div> 
    </div>

    <!-- choose filter -->
    <div class="row justify-content-center margin-top">
    <div>
      <!-- class="row justify-content-center" -->
        <!-- <h6 class="thick">Filter</h6> -->

        <label for="filter_select">Filter</label>
        <select id="filter_select" class="width-auto form-control" v-on:change="changeFilter" v-model="filterSelection" required>
          <!-- input -->
          <option disabled value="">Select</option>
          <option value="all" selected="selected">Upcoming</option>
          <!-- <option value="all">Upcoming</option> -->
          <option value="category">Category</option>
          <option value="date">Date</option>
          <!-- <option value="search">search</option> -->
        </select>
    </div>
    </div>

    <div v-show="action === 'all'">
		 <v-container fluid grid-list-lg>
			<v-layout row wrap>
        <!-- <v-flex xs12 v-for="(event, eventIndex) in eventData" :key="eventIndex"> -->
          <v-flex xs12 v-for="index in 18" :key="index">
					<v-card :color="eventData[index].color">
						<v-card-title primary-title>
							<div class="headline">{{eventData[index].title}}</div>
								</v-card-title>
                    <v-expansion-panel>
                      <v-expansion-panel-content>
                        <div slot="header"> {{ eventData[index].dateDisplay }} </div>
                        <v-card>
                          <v-card-text> {{eventData[index].content }} </v-card-text>
                          <social-sharing v-bind:url="eventData[index].link" inline-template>
                            <div id="fb_link">
                              <network network="facebook">
                                <!-- <i class="fa fa-facebook"></i>Share to Facebook -->
                              <p>Share to Facebook</p>
                              </network>
                            </div>
                           </social-sharing>

                          <add-to-calendar :title="eventData[index].title" 
                                      :start="new Date(eventData[index].pubDate)"
                                      :end="new Date((new Date(eventData[index].pubDate)).setHours((new Date(eventData[index].pubDate)).getHours() + 1))"
                                      :details="eventData[index].description" 
                                      inline-template> 
                            <div>
                              <google-calendar id="google-calendar">
                                <p>Add to Google calendar</p>
                              </google-calendar>
                            </div>
                          </add-to-calendar>

                        </v-card>
                      </v-expansion-panel-content>
                    </v-expansion-panel>

									</v-card>
								</v-flex>
								
							</v-layout>
						</v-container>
    </div>



    <!-- filter by date -->
    <div v-show="action === 'date'">

    <div class="row justify-content-center margin-top">
    <div>

      <v-date-picker class="input" v-model="picker" @input="filterByDate"></v-date-picker>

      <!-- <datetime v-model="dateSelection" @input="filterByDate" class="input form-control"></datetime> -->

      <!-- <datepicker v-model="dateSelection" name="uniquename" @input="filterByDate" wrapper-class="fullscreen-when-on-mobile" class="input form-control" placeholder="Select date"></datepicker>   -->

      <!-- class="input width-auto form-control" -->
      <!-- wrapper-class="fullscreen-when-on-mobile" -->

    </div>
    </div> 

		 <v-container fluid grid-list-lg>
			<v-layout row wrap>
				<v-flex xs12 v-for="(event, eventIndex) in eventsFilteredDate" :key="eventIndex">
					<v-card :color="event.color">
						<v-card-title primary-title>
							<div class="headline">{{event.title}}</div>
								</v-card-title>
                    <v-expansion-panel>
                      <v-expansion-panel-content>
                        <div slot="header"> {{ event.dateDisplay }} </div>
                        <v-card>
                          <v-card-text> {{event.content }} </v-card-text>
                          <social-sharing v-bind:url="event.link" inline-template>
                            <div>
                              <network network="facebook">
                              <p class="link">Share to Facebook</p>
                              </network>
                            </div>
                           </social-sharing>

                          <add-to-calendar :title="event.title" 
                                      :start="new Date(event.pubDate)"
                                      :end="new Date((new Date(event.pubDate)).setHours((new Date(event.pubDate)).getHours() + 1))"
                                      :details="event.description" 
                                      inline-template> 
                            <div>
                              <google-calendar id="google-calendar">
                                <p>Add to Google calendar</p>
                              </google-calendar>
                            </div>
                          </add-to-calendar>

                        </v-card>
                      </v-expansion-panel-content>
                    </v-expansion-panel>

									</v-card>
								</v-flex>
								
							</v-layout>
						</v-container>
    </div>



    <!-- filter by category -->
    <div v-show="action === 'category'">
    <div class="row justify-content-center margin-top">
    <div>
      <label for="category_select" >Select</label>
      <select id="category_select" class="width-auto form-control" v-on:change="filterByCategory" v-model="categorySelection">
            <!-- input -->
            <!-- <option disabled value="">Select category</option> -->
            <option value="Activities/Service">Service</option>
            <option value="Activities/Sports">Sports</option>
            <option value="Activities/Talent">Talent</option>
            <option value="Activities/Fitness">Fitness</option>
            <option value="Activities/Social">Social</option>
            <option value="Activities/Outdoor">Outdoor</option>
            <option value="Activities/Wellness">Wellness</option>
            <option value="Activities/Life Skills">Life Skills</option>
      </select>   
    </div>   
    </div>
		 <v-container fluid grid-list-lg>
			<v-layout row wrap>

				<v-flex xs12 v-for="(event, eventIndex) in eventsFilteredCategory" :key="eventIndex">
					<v-card :color="event.color">
						<v-card-title primary-title>
							<div class="headline">{{event.title}}</div>
								</v-card-title>
                    <v-expansion-panel>
                      <v-expansion-panel-content>
                        <div slot="header"> {{ event.dateDisplay }} </div>
                        <v-card>
                          <v-card-text> {{event.content }} </v-card-text>
                          <social-sharing v-bind:url="event.link" inline-template>
                            <div>
                              <network network="facebook">
                              <p class="link">Share to Facebook</p>
                              </network>
                            </div>
                           </social-sharing>

                          <add-to-calendar :title="event.title" 
                                      :start="new Date(event.pubDate)"
                                      :end="new Date((new Date(event.pubDate)).setHours((new Date(event.pubDate)).getHours() + 1))"
                                      :details="event.description" 
                                      inline-template> 
                            <div>
                              <google-calendar id="google-calendar">
                                <p>Add to Google calendar</p>
                              </google-calendar>
                            </div>
                          </add-to-calendar>

                        </v-card>
                      </v-expansion-panel-content>
                    </v-expansion-panel>

									</v-card>
								</v-flex>
								
							</v-layout>
						</v-container>
    </div>





    <!-- filter by keyword -->
    <div v-show="action === 'search'">
<!-- 
    <div class="row justify-content-center margin-top">
    <div>

      <form class="input width-auto form-control" @submit.prevent="filterBySearch">
        <input placeholder="keyword" v-model="search.keyword"/>
        <button type="submit">Search</button>   @click="filterBySearch()" 
      </form> 
    </div> 
    </div> -->

		 <v-container fluid grid-list-lg>
			<v-layout row wrap>
				<v-flex xs12 v-for="(event, eventIndex) in eventsFilteredSearch" :key="eventIndex">
					<v-card :color="event.color">
						<v-card-title primary-title>
							<div class="headline">{{event.title}}</div>
								</v-card-title>
                    <v-expansion-panel>
                      <v-expansion-panel-content>
                        <div slot="header"> {{ event.dateDisplay }} </div>
                        <v-card>
                          <v-card-text> {{event.content }} </v-card-text>
                          <social-sharing v-bind:url="event.link" inline-template>
                            <div>
                              <network network="facebook">
                              <p class="link">Share to Facebook</p>
                              </network>
                            </div>
                           </social-sharing>

                          <add-to-calendar :title="event.title" 
                                      :start="new Date(event.pubDate)"
                                      :end="new Date((new Date(event.pubDate)).setHours((new Date(event.pubDate)).getHours() + 1))"
                                      :details="event.description" 
                                      inline-template> 
                            <div>
                              <google-calendar id="google-calendar">
                                <p>Add to Google calendar</p>
                              </google-calendar>
                            </div>
                          </add-to-calendar>

                        </v-card>
                      </v-expansion-panel-content>
                    </v-expansion-panel>

									</v-card>
								</v-flex>
								
							</v-layout>
						</v-container>

  </div>
</div>  
</template>

<script>
  const rssAPI = 'https://wt-c2bde7d7dfc8623f121b0eb5a7102930-0.sandbox.auth0-extend.com/getRss?url=';
  const colors = ["indigo","blue","cyan","light-blue","teal","light-green","blue-grey"];

  import Datepicker from 'vuejs-datepicker';

  import moment from 'moment';  

  import Vuetify from 'vuetify';

  // Use a requirement statement to import library in .vue file:
  // https://stackoverflow.com/questions/43608457/
  var SocialSharing = require('vue-social-sharing');

  // Access a library with Vue.use()
  // https://stackoverflow.com/questions/41076828/
  import Vue from 'vue'
  Vue.use(SocialSharing);

  var AddToCalendar = require('vue-add-to-calendar');
  Vue.use(AddToCalendar);

  import BootstrapVue from 'bootstrap-vue'
  Vue.use(BootstrapVue);
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'

  import vueHeadful from 'vue-headful';

  Vue.component('vue-headful', vueHeadful);

//   new Vue({
//     // your configuration
// });

  // import Datetime from 'vue-datetime'
  // // You need a specific loader for CSS files
  // import 'vue-datetime/dist/vue-datetime.css'
  // Vue.use(Datetime)

  export default {
    data () {
      return {
        date: new Date(),
        options: {
          format: 'd MMM YYYY',
          useCurrent: false,
        },
        action: 'all',
        eventData: [],
        filterSelection: '',
        dateSelection: '',
        categorySelection: '',
        search: {
          keyword: ''
        },
        eventsFilteredDate: [],
        eventsFilteredCategory: [],
        eventsFilteredSearch: [],
        msg: 'Welcome to Your Vue.js App',
        noArray: [],
        drawer:true,
			  showIntro:false,
			  addFeedDialog:false,
			  addURL: 'https://calendar.byui.edu/RSSFeeds.aspx?data=FBqjK%2fSDoNoXjLLIGz5Kea2SyIk%2bK80m9T8RR%2bZnYumBTZxQgj8tnUyxXbSmZzvpAtLMsRTG%2bPcnabTM4oKMbg%3d%3d',
			  urlError:false,
			  urlRules:[],
			  feeds:[],
        selectedFeed:null, 
        picker: new Date().toISOString().substr(0, 10)
      } 
    }, 
    created() {
		  // do single file components use the created hook?  
		  // what is 'this' at this point in the code?
		  this.loadData();
    },
    mounted() {
      let calendarScript = document.createElement('script');
      // let calendarScript2 = document.createElement('script');
      calendarScript.setAttribute('src', 'https://addevent.com/libs/atc/1.6.1/atc.min.js')
      // calendarScript2.setAttribute('src', '//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-5be25a4f49650ba7')
      document.head.appendChild(calendarScript);
      // document.head.appendChild(calendarScript2);
    },
    methods: { 
      changeFilter() { 
        this.action = this.filterSelection; 
        console.log("filterSection = " + this.filterSelection);
      }, 
      showSearchBar() {
        if (this.action !== 'date')
          return true;
        else 
          return false;
        // return (this.action === 'all' || this.action === 'category');
		  },
      filterByDate() { 
        var date_input_moment;
        console.log("filterByDate has been called");
        this.action = 'date';
        this.eventsFilteredDate = [];
        // date_input_moment = moment(this.dateSelection);
        date_input_moment = moment(this.picker);    
        console.log("picker")    
        console.log(this.picker);
        console.log("date_input_moment");
        console.log(this.date_input_moment);
        for(var event of this.eventData) 
        {
          console.log("event comparisons")
          console.log(event.pubDate)
          console.log(date_input_moment)
          if (event.pubDate.isSame(date_input_moment, 'day'))
            this.eventsFilteredDate.push({title: event.title, content: event.content, category: event.categories[0], pubDate: event.pubDate, dateDisplay: event.dateDisplay, color: event.color, link: event.link });
        }
      }, 
      filterByCategory() {
        console.log("filterByCategory called");
        console.log(this.categorySelection);  
        this.action = 'category';
        this.eventsFilteredCategory = [];
        for(var event of this.eventData) {
          //console.log(event.categories[0]);
          if (event.categories[0] === this.categorySelection) {
            this.eventsFilteredCategory.push({title: event.title, content: event.content, category: event.categories[0], pubDate: event.pubDate, dateDisplay: event.dateDisplay, color: event.color, link: event.link });
          }
        }
      },
      filterBySearch () {
        this.action = 'search';
        this.eventsFilteredSearch = [];
        // obtain data
        var nKeyword = this.search.keyword;
        var eventText = '';
        // empty input field
        this.search.keyword = '';
        if (nKeyword !== '') {
          // find relevant events
          for (var event of this.eventData) {
            eventText = event.title + ' ' + event.pubDate + ' ' + event.content;
            // add way tosearch all event fields
            if (eventText.toUpperCase().includes(nKeyword.toUpperCase())) {
              this.eventsFilteredSearch.push({title: event.title, content: event.content, category: event.category, pubDate: event.pubDate, dateDisplay: event.dateDisplay, color: event.color, link: event.link });
            }
          }
          if (this.eventsFilteredSearch.length == 0)
          {
            alert('No events found containing \'' + nKeyword + '\'');  
            console.log('No events found containing \'' + nKeyword + '\'');         
          }
        }
      },
      loadData() {
			  this.urlError = false;
			  this.urlRules = [];
        // code purposely left out from tutorial 
      
        fetch(rssAPI+encodeURIComponent(this.addURL)) 
        .then(res => res.json())
				.then(res => {
					// ok for now, assume no error, cuz awesome
					this.addURL = '';

					//assign a color first
					res.feed.color = colors[this.feeds.length % (colors.length-1)];

					// ok, add the items (but we append the url as a fk so we can filter later)
					res.feed.items.forEach(item => {
						item.feedPk = this.addURL;           // What is this addURL?
            item.color = res.feed.color;
            item.pubDate = moment(item.pubDate); 
            item.dateDisplay = moment(item.pubDate).format('ddd MMM Do YYYY');
            console.log(item);              
						this.eventData.push(item);      
					});          

					// delete items
					delete res.feed.items;

					// add the original rss link
					res.feed.rsslink = this.addURL;

					this.feeds.push(res.feed);
					this.addFeedDialog = false;

					//always hide intro
					this.showIntro = false;

					//persist the feed, but not the items
					this.storeFeeds();
				});
      },
      storeFeeds() {
			  console.log('calling storeFeeds');
			  localStorage.setItem('feeds', JSON.stringify(this.feeds));
		  }
    }, 
    components: {
      //datePicker
      Datepicker
    }
  }
</script>
      
<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
  text-align: center;
}

.events {
  display: block;
  width: 400px;
  margin: auto;
  align-content: center;
}

.card {
  padding: 14px;
  box-shadow: 0px 0px 2px grey;
  margin: 1rem auto;
  border-style: solid;
}

.addeventatc {
  margin: 1rem auto;
}

.input {
  margin: 1rem;
}

.dataFilter {
  margin-top: 1rem auto;
}

button.delete  {
  background-color: pink;
}

button {
  margin-right: 1rem;
}

v-expansion-panel-content, select, input, input:active, input:focus, input:focus-within, input:hover, input:visited {
    font-size: 16px!important;
}

.margin-top {
  margin-top: 1rem;
}

#fb_link p {
 color: #0000EE;
}

.btn {
  border: 1px solid black;
  background-color: white;
  color: black;
  // padding: 0.1 rem 0.1 rem;
  font-size: 14px;
  cursor: pointer;
}

.success {
  border-color: #4055B2; // #4CAF50;
  color: #4055B2;
}

.border-input {
  border: 1px solid gray;
  border-radius: 4px;
  margin: 10 rem;
}

.inline {
  display: inline-block;
}

.wrapper {
  text-align: center;
  display: inline-block;
  position: absolute;
  top: 50%;
  left: 50%;
}

.form-control-custom {
  display: block;
    width: auto;
    height: calc(2.25rem + 2px);
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-top-color: rgb(206, 212, 218);
    border-top-style: solid;
    border-top-width: 1px;
    border-right-color: rgb(206, 212, 218);
    border-right-style: solid;
    border-right-width: 1px;
    border-bottom-color: rgb(206, 212, 218);
    border-bottom-style: solid;
    border-bottom-width: 1px;
    border-left-color: rgb(206, 212, 218);
    border-left-style: solid;
    border-left-width: 1px;
    border-image-source: initial;
    border-image-slice: initial;
    border-image-width: initial;
    border-image-outset: initial;
    border-image-repeat: initial;
    border-radius: 0.25rem;
    border-top-left-radius: 0.25rem;
    border-top-right-radius: 0.25rem;
    border-bottom-right-radius: 0.25rem;
    border-bottom-left-radius: 0.25rem;
}

// .vdp-datepicker {
    // &.fullscreen-when-on-mobile {
    //   .fullscreen-when-on-mobile {
    //     @media (max-width: 767px) {
    //         position: static;
    //         .vdp-datepicker__calendar {
    //             position: fixed;
    //             top: 50%;
    //             left: 5%;
    //             margin-top: -25%;
    //             width: 90%;
    //         }
    //     }
    // }
// }

</style>
