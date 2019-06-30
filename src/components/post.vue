<template>
  <div id='posts-wrapper'>
    <hr/>
    <h2>みんなの投稿</h2>
    <b-button v-on:click='show_all_card' variant="outline-primary">ぜんぶ</b-button>
    <b-button v-on:click='show_news_card' variant="outline-primary" style='margin:0px 10px;'>お知らせ</b-button>
    <b-button v-on:click='show_shopping_card' variant="outline-primary">買い物</b-button>
    <transition-group name='news-card-transition'>
      <b-card
        img-alt="Image"
        img-src="https://sugitashinjiro.fun/wp-content/uploads/2019/06/6b9021eab5eae199f24949ec18a73317.png"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="post-card"
        v-for='post in posts'
        v-bind:key="post.id"
        v-if='post.cont_type=="news"'
        v-show='card_show_news'
      >
        <b-card-text>
          <p style='font-size:80%;color:grey;'>{{post.name}}</p>
          <p>{{post.cont}}</p>
        </b-card-text>
      </b-card>
    </transition-group>
    <transition-group name='shopping-card-transition'>
    <b-card
        img-alt="Image"
        img-src="https://sugitashinjiro.fun/wp-content/uploads/2019/06/1afdf9c70417307de37a10f8cef87096.png"
        img-top
        tag="article"
        style="max-width: 20rem;"
        class="post-card"
        v-for='post in posts'
        v-bind:key="post.id"
        v-if='post.cont_type=="shopping"'
        v-show='card_show_shopping'
      >
        <b-card-text>
          <p style='font-size:80%;color:grey;'>{{post.name}}</p>
          <p>{{post.cont}}</p>
        </b-card-text>
      </b-card>
    </transition-group>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'form-wrapper',
  data() {
    return {
      posts: [],
      card_show_news: true,
      card_show_shopping: true,
    }
  },
  created:
  function() {
    firebase.firestore().collection("posted_data").where("id", ">=", "1")
      .onSnapshot(function(querySnapshot) {
          querySnapshot.forEach(function(doc) {

            var id_ary　= new Array();
            this.posts.forEach(function(post){
              id_ary.push(post.id);
            });

            if (id_ary.indexOf(doc.data().id) == -1) {
              this.posts.push(doc.data());
            }

          }.bind(this));
      }.bind(this));
  },
  methods: {
    show_all_card: function() {
      this.card_show_news = true;
      this.card_show_shopping = true;
    },
    show_news_card: function() {
      this.card_show_news = true;
      this.card_show_shopping = false;
    },
    show_shopping_card: function() {
      this.card_show_shopping = true;
      this.card_show_news = false;
    }
  }
}
</script>

<style>
.post-card {
  margin: 30px auto;
}

.news-card-transition-enter-active, .news-card-transition-leave-active {
  transition: opacity 1s, transform 1s;
}
.news-card-transition-enter {
  opacity: 0;
  transform: translateX(-70px);
}
.news-card-transition-leave-to {
  opacity: 0;
  transform: translateX(70px);
}

.shopping-card-transition-enter-active, .shopping-card-transition-leave-active {
  transition: opacity 1s, transform 1s;
}
.shopping-card-transition-enter {
  opacity: 0;
  transform: translateX(-70px);
}
.shopping-card-transition-leave-to {
  opacity: 0;
  transform: translateX(70px);
}
</style>
