<style lang='scss'>
    .image {
        height: 10rem;
        width: 10rem;
    }
</style>
<template>
    <div>
        <et-nav :user="user"></et-nav>
        <div class="container">
            <div class="row">
                <div class="col-xs-12 col-md-12">
                    <h2>{{ seller.name }}</h2>
                    <img v-bind:src="seller.image" class="image img-responsive img-circle" alt="Responsive image">
                    <a v-bind:href="'https://twitter.com/' + seller.twitter">@{{ seller.twitter }}</a>
                </div>
            </div><hr>
            <div class="row">
                <div class="col-xs-12 col-md-4" v-for="post in posts">
                    <div class="thumbnail">
                        <img :src="post.image" alt="...">
                        <div class="caption">
                            <h3>{{ post.title }}</h3>
                            <p>Seller: {{ post.seller }}</p>
                            <p>Posted: {{ post.posted | date }}</p>
                            <a v-bind:href="'/posts/' + post.id"  class="btn btn-success" role="button">View Post</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
  import axios from 'axios';
  import Moment from 'moment';
  import { orderBy } from 'lodash';
  import EtNav from './nav.vue';
  import auth from '../js/auth.js';

  export default {
    created() {
      this.getSeller();
      this.checkAuth();
    },

    data() {
      return {
        seller: '',
        posts: [],
        auth: auth,
        user: auth.user,
      };
    },

    methods: {
      getSeller() {
        axios.get(`/api/elephpants/sellers/${this.$route.params.id}`).then(response => {
          this.seller = response.data.data;
        }).catch(error => {
          console.error(error);
        });
      },

      checkAuth() {
        return auth.check();
      },
    },

    watch: {
      seller(value) {
        console.log(value);
        this.posts = value.posts.data;
      }
    },

    filters: {
      date(value) {
        return Moment(value.date).format('MM/DD/YYYY');
      },
    },

    components: {
      EtNav,
    },
  };
</script>