<template>
  <div>
    <HomeHeader @selectMenu="select"></HomeHeader>
    <div class="content"
         v-infinite-scroll="loadMore"
         :infinite-scroll-disabled="hasMore"
         infinite-scroll-distance="10"
        >
      <div ref="swiper" v-if="getSliders.length">
        <mt-swipe :auto="4000">
          <mt-swipe-item v-for="slider in getSliders">
            <img :src="slider" @load="load" ref="oImg">
          </mt-swipe-item>
        </mt-swipe>
      </div>
      <div class="home-lessons"  >
        <div class="lesson" v-for="item in getLessons">
          <img :src="item.url"/>
          <p>{{item.title}}</p>
          <p>{{item.price}}</p>
        </div>
        <div v-if="!hasMore" class="line">我是有底线的</div>
      </div>
    </div>
  </div>
</template>
<script>
  import HomeHeader from './HomeHeader.vue';
  import {mapActions,mapGetters,mapMutations} from 'vuex';
  import * as Types from '../../store/mutation-types'
    export default {
        data(){
            return {msg: 'hello'}
        },
        created(){
          this[Types.GET_SLIDERS]();
        },
        methods: {
          ...mapMutations([Types.CHANGE_TYPE,Types.CLEAR_LESSONS]),
           ...mapActions([Types.GET_SLIDERS,Types.GET_LESSONS]),
            select(type){
              this[Types.CLEAR_LESSONS]();
              this[Types.CHANGE_TYPE](type);
              this[Types.GET_LESSONS]();
               // 选择的类型
            },
            loadMore(){
              this[Types.GET_LESSONS]();
            },
            load(e){
                if(!this.loaded){
                  this.$nextTick(()=>{
                    this.$refs.swiper.style.height =this.$refs.oImg[0].offsetHeight + 'px';
                  });
                  this.loaded=true;
                }
            }
        },
        computed: {
          ...mapGetters(['getSliders','getLessons','hasMore'])
        },
        components: {HomeHeader}
    }
</script>
<style scoped lang="less">
img{max-width: 100%}
.content{
  position: fixed;
  top:56px;
  bottom: 54px;
  left:0;
  right:0;
  overflow: scroll;
}
.home-lessons{
  padding: 10px 7.5px;
  .lesson{
    border-radius: 5px;
    box-shadow: 1px 1px 2px 1px #c5c5c5,-1px -1px 2px 1px #c5c5c5;
    overflow: hidden;
    text-align: center;
    margin: 18px 0;
    img{
      height:140px;
      border-radius: 5px 5px 0 0;
    }
    p{
      line-height: 200%;
    }
    p:nth-child(2){
      color:#bbbbbb;
    }
    p:nth-child(3){
      color:#ed3a3a;
    }
  }
  .load-more{
    height:30px;
    line-height: 30px;
    border-radius: 5px;
    text-align: center;
    background-color: lightskyblue;
  }
  .line{
    width: 100%;
    text-align: center;
    line-height: 35px;
  }
}
</style>
