<template>
  <div id="app">
    <basic-vue-chat :title="'UL'" :initial-feed="feed" :new-message="message"   @newOwnMessage="onNewOwnMessage"/>
   
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import BasicVueChat from "basic-vue-chat";

export default {
  name: "app",
  components: {
    HelloWorld,
    BasicVueChat
  },
  mounted () {
   this.$mqtt.subscribe('VueMqtt/publish2')
  },
  data: function() {
    var feed = [
      {
        id: 1,
        author: "UL",
        imageUrl: "http://path/to/image",
        contents: "hi there welcome to ULTIMATE LEADS WELL",
        date: "16:30"
      }      
    ];

    return {
      feed
    };
  },
  mqtt: {
    'VueMqtt/publish2': function(val) {
      console.log(val.toString());
       var d ={};
      d['id']= 0;
      d['author']= "ULs"; 
      d['imageUrl']= "http://path/to/image";
      d['contents']= val.toString() ;
      d['date']="16:30";
      this.feed.push(d);
      console.log("test", this.feed);
      
      
    },
    'param/#': function(val) {
      console.log('param/#')
    },
    'param/param/param/test': function(val) {
      console.log('param/param/param/test')
    },
    'template/+' (data, topic) {
      if (topic.split('/').pop() === '12345') {
        console.log('topic:', 'template/12345')
      }
    },
    'template/+/param/param' (data, topic) {
      if (topic.split('/')[1] === '12345') {
        console.log('topic:', 'template/12345/param/param')
      }
    }
  },
  methods: {
    publish1 :function (event)  {
       this.$mqtt.publish('VueMqtt/publish1', 'message to Sub1')
    },
    onNewOwnMessage :function(e){
       this.$mqtt.publish('VueMqtt/publish1', e);
    },
    newMessage:function(e){
      console.log(e);
    },
    message:function(e){
      
    }
    
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
