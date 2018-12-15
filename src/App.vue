<template>

  <div id="app">
    <h1>Event List</h1>

    <!-- choose filter -->
    <div class="row justify-content-center margin-top">
    <div>
      <!-- class="row justify-content-center" -->
        <select class="width-auto form-control" v-on:change="changeFilter" v-model="filterSelection">
          <!-- input -->
          <option disabled value="">Select Filter</option>
          <option value="all">All</option>
          <option value="search">search</option>
          <option value="category">category</option>
          <option value="date">date</option>
        </select>
    </div>
    </div>

    <div v-show="action === 'all'">
		 <v-container fluid grid-list-lg>
			<v-layout row wrap>
				<v-flex xs12 v-for="(event, eventIndex) in eventData" :key="eventIndex">
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
                              <p>Share</p>
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

    <!-- filter by date -->
    <div v-show="action === 'date'">
      <datepicker v-model="dateSelection" name="uniquename" @input="filterByDate" placeholder="Select date"></datepicker>     
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
                              <p>Share</p>
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
      <select class="input width-auto form-control" v-on:change="filterByCategory" v-model="categorySelection">
            <!-- input -->
            <option disabled value="">Select category</option>
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
                              <p>Share</p>
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
      <form class="input" @submit.prevent="filterBySearch">
        <input placeholder="keyword" v-model="search.keyword"/>
        <button type="submit">Search</button>  <!-- @click="filterBySearch()" -->
      </form> 

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
                              <p>Share</p>
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
			  selectedFeed:null
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
      filterByDate() { 
        var date_input_moment;
        console.log("filterByDate has been called");
        this.action = 'date';
        this.eventsFilteredDate = [];
        date_input_moment = moment(this.dateSelection);
        console.log(this.dateSelection);
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
            item.dateDisplay = moment(item.pubDate).format('MMM Do YY');
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
      
<style>
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
</style>
