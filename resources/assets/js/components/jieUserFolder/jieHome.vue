<template>
   <v-card  app height="100%" width="100%" style="position:absolute; bottom:0" flat class="newsfeedScroll pa-0 ma-0 transparent scrollbar-primary "> 
      <v-layout class="mt-2 justify-start justify-space-arround">
         <v-flex class=" xs12 sm12 md8 lg7  mr-2 mt-2 ml-3 ">
            <v-card flat style="border:1px #E0E0E0 solid;border-radius:2px" class="mb-1 jieSvgBg1  " >
               <v-layout wrap color="transparent" :class="whatisClass" class="px-2" >
                  <v-flex xs12  class="mb-1" >
                     <v-layout>
                        <v-btn
                           color="grey lighten-3"
                           round
                           depressed
                           small
                           style="font-size:12px" class="font-weigth-bold textfm1 textDefault"
                        >
                           <v-icon dark class="mr-1" color="blue" style="font-size:15px">mdi-pencil</v-icon>
                           Make Post
                        </v-btn>
                        <v-btn
                           color="grey lighten-3"
                           round
                           depressed
                           small
                           style="font-size:12px" class="font-weigth-bold textfm1 textDefault"
                        >
                           <v-icon dark class="mr-1" color="brown" style="font-size:15px">mdi-image-album</v-icon>
                           Photo/Album
                        </v-btn>
                        <v-btn
                           color="grey lighten-3"
                           round
                           depressed
                           small
                           style="font-size:12px" class="font-weigth-bold textfm1 textDefault"
                        >
                           <v-icon dark class="mr-1" color="green" style="font-size:15px">mdi-poll</v-icon>
                           Poll Voting
                        </v-btn>
                        <v-spacer></v-spacer>
                        <v-btn
                           icon
                           color="grey lighten-3"
                           round
                           depressed
                           small
                           style="font-size:12px" class="font-weigth-bold textfm1 textDefault"
                        >
                           <v-icon dark  color="grey darken-4" style="font-size:17px">mdi-dots-horizontal</v-icon>
                        </v-btn>
                     </v-layout>

                  </v-flex>
                 
                  <v-flex  xs1 class="mt-3">
                     <v-btn @click="profileMenu" color="" icon style="height:34px !important; width:34px !important;margin-top:-5px" class=" jieleftNav"  flat>
                        <v-badge color="white"  overlap class="jieBadgeNews">
                           <!-- <span  slot="badge" class="" style="font-size:16px; border-radius: 50%; border: 4.5px solid #7CB342 ;"></span> -->
                           <v-avatar class="mr-2 " color="grey lighten-3" size="35">
                              <img :src="userData.photoUrl" alt="">
                           </v-avatar>
                        </v-badge>
                     </v-btn> 
                  </v-flex>
                  <v-flex :class="whatisFlex">
                  
                     <v-textarea 
                        v-model="postedData.message"
                        class="jiew textfm1"
                        row-height="10"
                        style="font-size:16px !important"
                        placeholder="What is your main focus for today?"
                        @click="whatisFunctionMethod"
                        @blur="whatisFunctionMethodFalse"
                        flat solo
                        auto-grow
                     ></v-textarea>
                  </v-flex>
                  <v-flex v-if="!whatisFunction" style="margin-top:1px">
                     <!-- <p class="text-xs-center"> -->
                     <span style="font-size:28px" class="textfm2">{{timeDisplay}}</span>
                     <br>
                     <span class="textfm2 black--text" style="font-size:15px"> {{greet}}</span>
                     <!-- </p> -->
                  </v-flex>

                  <v-flex xs12  v-if="whatisFunction" >
                     <v-btn small block :disabled="postedDataNews"  depressed color="indigo" @click="makePost(userData)" class="white--text caption textDefault"> Post </v-btn>
                  </v-flex>
               </v-layout>
               <!-- <v-progress-linear  v-if="whatisFunction"  height="2" style="margin:0px !important" color="grey" :indeterminate="true"></v-progress-linear> -->
               <v-layout>
                  <v-flex  xs1 v-for="newsfeed in newsfeeds" :key="newsfeed['.key']" v-if="newsfeed.userId != userData['ckcm-network_token_id']"  class="pa-2">
                           <v-btn color="indigo" icon @click="test" class="">
                              <v-avatar class="" color="grey" size="34px">
                                 <img :src="newsfeed.photoUrl" alt="">
                              </v-avatar>
                           </v-btn>
                  </v-flex>
               </v-layout>
               <!-- </v-container> -->
            </v-card>
            <v-card  style="border:1px #E0E0E0 solid;border-radius:2px" v-for="newsfeed in newsfeeds " :key="newsfeed['.key']" flat class="jieSvgBg1" >
               <!-- <v-progress-linear active height="2" style="margin:0px !important" color="grey lighten-2" :indeterminate="false"></v-progress-linear> -->
               <v-layout wrap white class=" py-2" >
                  <v-flex xs12 class="mx-2">
                     <v-layout>
                     <v-btn @click="profileMenu" color="" icon style="height:34px !important; width:34px !important" class=" jieleftNav"  flat>
                        <v-badge color="white"  overlap class="jieBadgeNews">
                           <span  slot="badge" class="" style="font-size:16px; border-radius: 50%; border: 4.5px solid #7CB342 ;"></span>
                           <v-avatar class="mr-2 " color="grey lighten-3" size="35">
                              <img :src="newsfeed.photoUrl" alt="">
                           </v-avatar>
                        </v-badge>
                     </v-btn> 
                     <div class="mt-2 ">
                     <p @click="profileMenuFriend(newsfeed.displayName)" style="font-size:15px" class="aJie mb-0 indigo--text text--darken-4 font-weight-bold textfm1">{{newsfeed.displayName}} </p>
                     <p style="margin-top:-5px;font-size:13px" class="grey--text textfm2">
                        <!-- {{ newsfeed.timestamp | moment("dddd, MMMM Do YYYY") }}| -->
                        <span> <timeago :auto-update="60" :datetime="newsfeed.timestamp"></timeago></span>
                     </p>
                     </div>

                     <v-spacer></v-spacer>
                     <v-menu style="margin-top:-27px;" offset-y nudge-left="80"> 
                          <v-btn
                           slot="activator"
                           icon
                           color="grey lighten-4"
                           round
                           depressed
                           small
                           style="font-size:12px" class="font-weigth-bold textfm1 textDefault"
                        >
                           <v-icon dark  color="grey darken-4" style="font-size:17px">mdi-dots-horizontal</v-icon>
                        </v-btn>
                        <v-list>
                        <v-list-tile
                           v-for="(item, index) in items"
                           :key="index"
                           @click="test"
                        >
                           <v-list-tile-title>{{ item.title }}</v-list-tile-title>
                        </v-list-tile>
                        </v-list>
                     </v-menu>
                      
                     </v-layout>

                  </v-flex>

                  <v-flex class="mx-3" xs12  >
                     <div v-if="newsfeed.data.message.length < 150" style="font-size:25px;" class="black--text textfm1">
                        {{newsfeed.data.message}}
                     </div>

                     <div v-else-if="newsfeed.data.message.length > 150" style="font-size:16px;" class="black--text textfm1">
                        {{newsfeed.data.message}}
                     </div>
                     <v-divider class="mt-3 grey lighten-3"> </v-divider>
                     <v-layout>
                        <v-flex xs3  >
                           <v-btn depressed color="transparent" class=" textDefault grey--text text--darken-1 textfm1" block>
                              <v-icon size="19" class="mr-1 --text text--darken-3" >mdi-thumb-up-outline</v-icon>
                              Like
                           </v-btn>
                        </v-flex>

                        <v-flex xs3  >
                           <v-btn depressed color="transparent" class=" textDefault grey--text text--darken-1 textfm1" block>
                              <v-icon size="19" class="--text text--darken-3 mr-1" >mdi-thumb-down-outline</v-icon>
                              Dislike
                           </v-btn>
                        </v-flex>

                        <!-- <v-flex xs3  >
                           <v-btn depressed color="transparent" class=" textDefault grey--text text--darken-1 textfm1" block>
                              <v-icon size="19" class="--text text--darken-3 mr-1" >mdi-thumb-down-outline</v-icon>
                              React
                           </v-btn>
                        </v-flex> -->

                        <v-flex xs3 >
                         <v-btn depressed color="transparent" class=" textDefault grey--text text--darken-1 textfm1" block>
                              <v-icon size="19" class="--text text--darken-3  mr-1" >mdi-comment-outline</v-icon>
                              Comment
                           </v-btn>
                        </v-flex>

                        <v-flex xs3 >
                           <v-btn depressed color="transparent" class=" textDefault grey--text text--darken-1 textfm1" block>
                              <v-icon size="20" class="--text text--darken-3 mr-1" >mdi-share-outline</v-icon>
                              Share
                           </v-btn>
                        </v-flex>

                     </v-layout>
                  </v-flex>
                  <v-flex xs12>
                     <v-divider class="grey lighten-3"></v-divider>
                  </v-flex>

                  <v-flex xs12 class="mx-2 ">
                     <!-- <li>{{newsfeed}}</li> -->
                     <v-flex xs12 v-for="commented in newsfeed.commented  " :key="commented['.key']">
                        <v-layout>
                           <div  class=" mt-3">
                              <v-btn @click="profileMenu" color="" icon style="height:34px !important; width:34px !important;margin-top:-5px" class=" jieleftNav"  flat>
                                 <v-badge color="white"  overlap class="jieBadgeNews">
                                    <v-avatar class="mr-2 " color="grey lighten-3" size="32">
                                       <img :src="commented.photoUrl" alt="">
                                    </v-avatar>
                                 </v-badge>
                              </v-btn> 
                           </div>
                        
                           <v-flex xs11  style="margin-top:12px;margin-left:-4px" >
                              <p style="width: auto !important" class=" jie3Commented">
                                 <router-link class="indigo--text aJie" to="/profile/jiecel.marianne">{{commented.displayName}}</router-link>
                                 {{commented.data}}
                              </p>
                              <div class="caption ml-2" style="margin-top:-13px; ">
                                 <span>
                                 <a style="font-size:13px;" class="aJie indigo--text mt-2  textfm1" @click="test"> Like </a> 
                                    <v-icon size="3px" class="ml-1" style="margin-top:9px;position:absolute">mdi-asterisk</v-icon>
                                 </span>
                                 <span class="ml-3">
                                 <a style="font-size:13px;" class=" aJie indigo--text mt-2  textfm1" @click="test"> Dislike </a> 
                                    <v-icon size="3px" class="ml-1" style="margin-top:9px;position:absolute">mdi-asterisk</v-icon>
                                 </span>
                                 <span class="ml-3">
                                 <a style="font-size:13px;" class=" aJie indigo--text mt-2  textfm1" @click="test"> Reply </a> 
                                    <v-icon size="3px" class="ml-1" style="margin-top:9px;position:absolute">mdi-asterisk</v-icon>
                                 </span>
                                 <span class="ml-3">
                                 <span style="font-size:13px;" class=" grey--text mt-2  textfm1" > 1h </span> 
                                 </span>

                              </div>
                           </v-flex>

                        </v-layout>
                     </v-flex>
                  </v-flex>
                  <v-flex v-if="newsfeed.someoneComment.someone == true" xs12 class="mx-5 px-2 mt-2">
                     <div xs12>
                        <v-progress-circular
                           :width="1"
                           :size="13"
                           color="indigo lighten-1"
                           indeterminate
                        ></v-progress-circular>
                        <span  class="caption font-weight-thin grey--text textfm1">Someone is typing...</span>
                     </div>
                  </v-flex>
                  <v-flex xs12 class="mx-2">
                     <v-layout>
                        <v-card flat xs1 class=" mt-3">
                           <v-btn @click="profileMenu" color="" icon style="height:34px !important; width:34px !important;margin-top:-5px" class=" jieleftNav"  flat>
                              <v-badge color="white"  overlap class="jieBadgeNews">
                                 <!-- <span  slot="badge" class="" style="font-size:16px; border-radius: 50%; border: 4.5px solid #7CB342 ;"></span> -->
                                 <v-avatar class="mr-2 " color="grey lighten-3" size="32">
                                    <img :src="userData.photoUrl" alt="">
                                 </v-avatar>
                              </v-badge>
                           </v-btn> 
                        </v-card>

                        <!-- comment area -->
                       
                        <v-flex  xs11 style="margin-top:-7px;margin-left:-5px">
                           <v-form @submit.prevent="commentPost(newsfeed['.key'], userData, newsfeed.commentText)">
                              <v-text-field  
                                 background-color="grey lighten-5"
                                 single-line
                                 solo
                                 hint="Press Enter to comment"
                                 flat
                                 height="32"
                                 full-width
                                 v-model="newsfeed.commentText"
                                 :loading="false"
                                 placeholder="Write something here.."
                                 @blur="blurPost(newsfeed['.key'], userData, newsfeed.commentText)"
                                 @input="inputPost(newsfeed['.key'], userData, newsfeed.commentText)"
                                 @click:append="commentPost(newsfeed['.key'], userData, newsfeed.commentText)"
                                 append-icon="mdi-send"
                                 style="font-size:13px"
                                 class="font-weight-thin-light jie3 textfm1  "
                              ></v-text-field>
                           </v-form>
                        </v-flex>
                        <!-- <v-flex style="margin-top:2px;margin-left:-5px">
                           <v-btn icon>
                              <v-icon class="grey--text text--lighten-1">
                                 mdi-camera
                              </v-icon>   
                           </v-btn>                           
                        </v-flex> -->
                     </v-layout>
                  </v-flex>

               <!-- <v-progress-linear active height="2" style="margin:0px !important" color="red lighten-2" :indeterminate="false"></v-progress-linear> -->
                  <!-- <v-flex class="jieSvgBg1" xs12>
                     footer
                  </v-flex> -->
               </v-layout>

            </v-card>
            
            <v-card class="my-2" >
               <v-card-media
                  height=""
               >
                  <v-container fill-height fluid>
                     <v-layout fill-height>
                     <v-flex>
                        <v-btn small block @click="scrollLoadNews">Load More</v-btn>
                     </v-flex>
                     </v-layout>
                  </v-container>
               </v-card-media>
            </v-card>
         </v-flex>
<!-- xs12 sm12 md8 lg7 -->
 <!-- xs0 sm0 md4 lg4 -->
         <v-flex  class="md4 lg4  hidden-sm-and-down  mr-2  " >
            <v-card flat class="grey lighten-4 my-2" height="50px">
            </v-card>
            <v-card flat  class="grey lighten-4 mb-2" height="150px">
            </v-card>
         </v-flex>
      </v-layout>
   </v-card>

</template>
<script>
import {newsfeedRef, db, app, order } from '../../firebase.js';


export default {  
   firebase: function () {
      return {
         newsfeeds: newsfeedRef,
      }
   },
   data: () => ({
      orderValue: '',
      timeDisplay: '',
      greet: '',
      worldTime: [],
      whatisFunction: false,
      whatisClass: "",
      bottomNav: 'recent',
      whatisFlex: 'xs7',
      userComment: '',
      timestamp: '',
      items: [
        { title: 'Op jie1' },
        { title: 'Op jie2' },
      ],
      postedData: { 
         message:'', 
         image:''
      },
  }),
   computed: {
      // newsfeedsReverse() {
      //    return this.$firebaseRefs.newsfeeds.split('').reverse().join('')
      // },
      userData() {
         return this.$store.getters.accountLoginData.user
      },
      firstname() {
         var getFn = this.$store.getters.accountLoginData.user.displayName
         var firstname = getFn.split(" ");
         return firstname[0];
      },
      postedDataNews() {
         if (this.postedData.message == "") return true
         this.postedData.message != ""
         return false
      },
   },
   methods: {
      scrollLoadNews() {
         alert("success")
         // this.$store.dispatch("scrollLimitNewsAdd");
      }, 
      test() {
         alert("success")
      },
      blurPost(id,user,commentText) {
         let vm = this
         db.ref(`Newsfeed/${id}/`).child('someoneComment').set({
            someone:false
         }, function(error) {
         if (error) {
            console.log(error)
            // The write failed...r
         } else {
            // Data saved successfully!
         }
         });
      },
      inputPost(id,user,commentText) {
         let vm = this
         db.ref(`Newsfeed/${id}/`).child('someoneComment').set({
            someone:true
         }, function(error) {
         if (error) {
            console.log(error)
            // The write failed...r
         } else {
            // Data saved successfully!
         }
         });
      },
      commentPost(id,user,commentText) {
         let vm = this
         db.ref(`Newsfeed/${id}/commented`).push().set({
            userId: user['ckcm-network_token_id'],
            displayName: user.displayName,
            data: commentText,
            photoUrl: user.photoUrl,
            timestamp: "",
         }, function(error) {
         if (error) {
            console.log(error)
            // The write failed...r
         } else {
            // Data saved successfully!
         }
         });
      },
      makePost(user) {
         let vm = this
            // timestamp: Date.now(),
         db.ref('Newsfeed/' ).push().set({
            userId: user['ckcm-network_token_id'],
            displayName: user.displayName,
            data: vm.postedData,
            photoUrl: user.photoUrl,
            timestamp: vm.timestamp,
            someoneComment: false,
            commentText: "",
            order: vm.orderValue + 1 
         }, function(error) {
         if (error) {
            console.log(error)
            // The write failed...r
         } else {
            db.ref('N_order/').set ({
               newsfeedOrder: vm.orderValue-1
            })
            vm.postedData.message = "";
            // Data saved successfully!
         }
         });
      },
      whatisFunctionMethod () {
         this.whatisClass = "mb-1 elevation-3"
         this.whatisFunction = true
         this.whatisFlex = "xs11"
      },
      whatisFunctionMethodFalse () {
         if(this.postedData.message == "") {
            this.whatisFunction = false
            this.whatisClass = ""
            this.whatisFlex = "xs7"
         } else {
            
         }
      },
      profileMenu () {
         const displayName= this.$store.getters.accountLoginData.user.displayName;
         const user = displayName.toLocaleLowerCase().replace(/[ ]/g, ".");
         this.$router.push({
            path: `/profile/${user}`,
         });
      },
      profileMenuFriend (displayName) {
         const user = displayName.toLocaleLowerCase().replace(/[ ]/g, ".");
         this.$router.push({
            path: `/profile/${user}`,
            });
      }
   },
   mounted: function() {

      this.$store.commit("leftnavDrawerOn");

      var date = new Date();
      let vm = this;
      // setTimeout(function() {
         setInterval(()=>{
            axios.get('https://api.ipgeolocation.io/ipgeo?apiKey=90a83c7326cc475f8048cf81362e1df0')
               .then((response) =>{
                  vm.worldTime=response.data
                  vm.timeDisplay = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('h:mma');
                  vm.timestamp =  vm.worldTime.time_zone.current_time
                  let hour = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('h');
                  let a = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('a');
                     if ( a == 'pm' && hour >= 1 && hour <= 5 || a == 'pm' && hour == 12) {
                        vm.greet = "Good afternoon , " +vm.firstname;
                     } else if ( a == 'pm' && hour >= 6) {
                        vm.greet = "Good evening , " + vm.firstname;
                     } else if (a == 'am' && hour >= 1 && hour <= 6) {
                        vm.greet = "Sayuha nimu , " + vm.firstname;
                     } else {
                        vm.greet = "Good morning , " + vm.firstname;
                     }
                     })
               .catch((err) => { 
                  console.log(err)
               })
               // MMMM Do YYYY ss,
         },60000)
      // }, (60 - date.getSeconds()) * 1000);
   },
   created() {

      document.title = "Christ the King Network";

      let vm = this;

      order.on("value",  function(snapshot) {
         vm.orderValue = snapshot.val();
      }, function (errorObject) {
         console.log("Naa ko sa created 1 :The read failed: " + errorObject.code);
      });


      axios.get('https://api.ipgeolocation.io/ipgeo?apiKey=90a83c7326cc475f8048cf81362e1df0')
         .then((response) =>{
            vm.worldTime=response.data
            vm.timeDisplay = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('h:mma');
            vm.timestamp =  vm.worldTime.time_zone.current_time
            let hour = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('h');
            let a = moment(vm.worldTime.time_zone.current_time).tz(vm.worldTime.time_zone.name).format('a');
               if ( a == 'pm' && hour >= 1 && hour <= 5 || a == 'pm' && hour == 12) {
                  vm.greet = "Good afternoon , " + vm.firstname;
               } else if ( a == 'pm' && hour >= 6) {
                  vm.greet = "Good evening , " + vm.firstname;
               } else if (a == 'am' && hour >= 1 && hour <= 6) {
                  vm.greet = "Sayuha nimu , " + vm.firstname;
               } else {
                  vm.greet = "Good morning , " + vm.firstname;
               }
         })
         .catch((err) => { 
            console.log(err)
         })
   
   },
   
}
</script>
