<template>

  <div id="app">
    <h1>Event List</h1>

    <!-- choose filter -->
    <div class="input">
        <select v-on:change="changeFilter" v-model="filterSelection">
          <option disabled value="">Select Filter</option>
          <option value="all">All</option>
          <option value="search">search</option>
          <option value="category">category</option>
          <option value="date">date</option>
        </select>
    </div>

    <social-sharing url="https://vuejs.org/"
                      title="The Progressive JavaScript Framework"
                      description="Intuitive, Fast and Composable MVVM for building interactive interfaces."
                      quote="Vue is a progressive framework for building user interfaces."
                      hashtags="vuejs,javascript,framework"
                      twitter-user="vuejs"
                      inline-template>
      <div>
        <network network="facebook">
          <i class="fa fa-facebook"></i> Facebook
        </network>
      </div>
    </social-sharing>

    <!-- Button code -->
    <div title="Add to Calendar" class="addeventatc">
        Add to Calendar
        <span class="start">12/07/2018 08:00 AM</span>
        <span class="end">12/07/2018 10:00 AM</span>
        <span class="timezone">America/Los_Angeles</span>
        <span class="title">Summary of the event</span>
        <span class="description">Description of the event</span>
        <span class="location">Location of the event</span>
    </div>

    <!-- start off showing all events -->
		<v-container fluid grid-list-lg>
			<v-layout row wrap>
			  <!-- this contains the list of event cards  -->
				<!-- what is v-flex? -->
				<v-flex xs12 v-for="(event, eventIndex) in eventData" :key="eventIndex">
					<v-card :color="event.feedColor">
						<v-card-title primary-title>
							<div class="headline">{{event.title}}</div>
								</v-card-title>
                    <v-expansion-panel>
                      <v-expansion-panel-content>
                        <div slot="header"> {{ event.dateDisplay }} </div>
                        <v-card>
                          <v-card-text> {{event.content }} </v-card-text>
                          <social-sharing url="https://vuejs.org/"
                          title="The Progressive JavaScript Framework"
                          description="Intuitive, Fast and Composable MVVM for building interactive interfaces."
                          quote="Vue is a progressive framework for building user interfaces."
                          hashtags="vuejs,javascript,framework"
                          twitter-user="vuejs"
                          inline-template>
                            <div>
                              <network network="facebook">
                              <i class="fa fa-facebook"></i> Facebook
                              </network>
                            </div>
                           </social-sharing>

                          <!-- Button code -->
                          <div title="Add to Calendar" class="addeventatc">
                              Add to Calendar
                              <span class="start">12/07/2018 08:00 AM</span>
                              <span class="end">12/07/2018 10:00 AM</span>
                              <span class="timezone">America/Los_Angeles</span>
                              <span class="title">Summary of the event</span>
                              <span class="description">Description of the event</span>
                              <span class="location">Location of the event</span>
                          </div>
                        </v-card>
                      </v-expansion-panel-content>
                    </v-expansion-panel>

									<!-- <v-card-text>
											{{event.content }}
										</v-card-text> -->

										<!-- <v-card-actions>
											<v-btn flat target="_new" :href="event.link">Read More...</v-btn>
										</v-card-actions> -->
									</v-card>
								</v-flex>
								
							</v-layout>
						</v-container>

    <!-- filter by date -->
    <div v-show="action === 'date'">
      <datepicker v-model="dateSelection" name="uniquename" @input="filterByDate"></datepicker>     
    </div>
    
    <div class="events">
      <div class="card" v-for="(event, eventIndex) in eventsFilteredDate" :key="eventIndex">
          <h4>{{ event.title }}</h4>
          <p>{{ event.content }}</p>
          <p>{{ event.category }}</p>          
          <p>{{ event.pubDate }}</p>
      </div>
    </div>

    <!-- filter by category -->
    <div class="dataFilter" v-show="action === 'category'">
      <select v-on:change="filterByCategory" v-model="categorySelection">
            <option disabled value=""></option>
            <option value="Activities/Service">Service</option>
            <option value="Activities/Sports">Sports</option>
            <option value="Activities/Talent">Talent</option>
            <option value="Activities/Fitness">Fitness</option>
            <option value="Activities/Social">Social</option>
            <option value="Activities/Outdoor">Outdoor</option>
            <option value="Activities/Wellness">Wellness</option>
            <option value="Activities/Life Skills">Life Skills</option>
      </select>      

      <div class="events">  
        <div class="card" v-for="(event, eventIndex) in eventsFilteredCategory" :key="eventIndex">
          <h4>{{ event.title }}</h4>
          <p>{{ event.content }}</p>
          <p>{{ event.category }}</p>          
          <p>{{ event.pubDate }}</p>
        </div>  
      </div>   
    </div>

    <!-- filter by keyword -->
    <div class="input" v-show="action === 'search'">
      <form @submit.prevent="filterBySearch">
        <input placeholder="keyword" v-model="search.keyword"/>
        <button type="submit">Search</button>  <!-- @click="filterBySearch()" -->
      </form> 

      <div class="events">  
        <div class="card" v-for="(event, eventIndex) in eventsFilteredSearch" :key="eventIndex"> 
          <h4>{{ event.title }}</h4>
          <p>{{ event.content }}</p>
          <p>{{ event.category }}</p>          
          <p>{{ event.pubDate }}</p>
        </div>  
      </div>    
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
		  // what is 'this', at this point in the code?
		  this.loadData();
    },
    mounted() {
      let calendarScript = document.createElement('script');
      calendarScript.setAttribute('src', 'https://addevent.com/libs/atc/1.6.1/atc.min.js')
      document.head.appendChild(calendarScript);
    },
    methods: { 
      changeFilter() { 
        this.action = this.filterSelection; 
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
            this.eventsFilteredDate.push({title: event.title, content: event.content, category: event.categories[0], pubDate: event.pubDate });
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
            this.eventsFilteredCategory.push({title: event.title, content: event.content, category: event.categories[0], pubDate: event.pubDate });
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
              this.eventsFilteredSearch.push({title: event.title, content: event.content, category: event.category, pubDate: event.pubDate });
            }
          }
        }

        // console.log(this.eventsFilteredSearch);
        // clear out this.search.keyword?
        // otherwise you hit an empty search box
        // and get same search results...
        // display relevant events

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
						item.feedPk = this.addURL;
            item.feedColor = res.feed.color;
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
  margin: 28px
}

.input {
  margin: 28px
}

.dataFilter {
  margin-top: 1rem auto;
}

input {
  margin-right: 1rem;
}

button.delete  {
  background-color: pink;
}

button {
  margin-right: 1rem;
}
</style>
