<template>
  <div id="parent">
    <div class="themeTitle">
      <div v-for="item in navigation" :style="item.style" @click="loadPage(item.className)" >
        <span :class="item.icon"></span>{{item.name}}
      </div>
    </div>
    <router-view class="loadRouter"></router-view>
    <div class="showBottomDiv" v-if="showSmallSong"></div>
  </div>
</template>

<script>
  import Bus from '../../global/bus.vue'
  import $ from 'jquery'
  export default {
    data(){
      return{
        showSmallSong:false,
        navigation:[
          {
            className:'lastNew',
            name:'最新',
            style:{
              borderBottom:'3px solid white'
            },
            iconName:'yinyue',
            icon:'iconfont icon-yinyueshi',
            path:'/navigation/lastNew'
          },
          {
            className:'recommended',
            name:'推荐',
            iconName:'tuijian',
            icon:'iconfont icon-tuijiankong',
            path:'/navigation/recommended'
          },
          {
            className:'search',
            name:'搜索',
            iconName:'sousuo',
            icon:'iconfont icon-sousuokong',
            path:'/navigation/search'
          },
          {
            className:'my',
            name:'我的',
            iconName:'wode',
            icon:'iconfont icon-wodekong',
            path:'/navigation/my'
          },
        ],
        theme:[
          {
            value:0,
            style:{
              background:'#054547',
            }
          },
          {
            value:1,
            style:{
              background:'rgb(116, 3, 73)',
            }
          },
          {
            value:2,
            style:{
              background:'rgb(95, 86, 54)',
            }
          },

        ]
      }
    },
    created(){
      this.loadPage(location.hash.split('#/navigation/')[1]);
      this.getBus();
    },
    updated(){
      this.getTheme();
    },
    methods:{
      getTheme(){
        const root = this;
        if(localStorage.getItem('songTheme')){
          let value = localStorage.getItem('songTheme');
          for(let i = 0;i<root.theme.length; i++){
            if(root.theme[i].value == value){
              $('.themeTitle').css('background',root.theme[i].style.background);
              $('.themeSmallSong').css('background','-webkit-linear-gradient(to right,'+root.theme[i].style.background+',#606266)');
              $('.themeSmallSong').css('background','-o-linear-gradient(to right, '+root.theme[i].style.background+' , #606266)');
              $('.themeSmallSong').css('background','-moz-linear-gradient(to right, '+root.theme[i].style.background+' , #606266)');
              $('.themeSmallSong').css('background','linear-gradient(to right, '+root.theme[i].style.background+' , #606266)');
            }
          }
        }else{
          $('.themeTitle').css('background','#054547');
          $('.themeSmallSong').css('background','-webkit-linear-gradient(to right,#054547,#606266)');
          $('.themeSmallSong').css('background','-o-linear-gradient(to right, #054547 , #606266)');
          $('.themeSmallSong').css('background','-moz-linear-gradient(to right, #054547 , #606266)');
          $('.themeSmallSong').css('background','linear-gradient(to right, #054547 , #606266)');
        }
      },
      getBus(){
        const root = this;
//        Bus.$on('acceptMessage',(msg) => {
//          root.showSmallSong = true;
//        })
      },
      //点击导航条按钮发生变化
      loadPage(e){
        const root = this;
        for(var i=0;i<root.navigation.length;i++){
          root.navigation[i].style={};
          root.navigation[i].icon='iconfont icon-'+root.navigation[i].iconName+'kong'
          if(root.navigation[i].className==e){
            root.navigation[i].style={
              borderBottom:'3px solid white'
            }
            root.navigation[i].icon=root.navigation[i].icon.split('kong')[0]+'shi';
            root.$router.push({path:root.navigation[i].path})
          }
        }

      },

    }
  }
</script>

<style scoped>
  .themeTitle{
    width: 100%;
    max-width: 520px;
    margin: 0 auto;
    position: fixed;
    top: 0;
    height: 80px;
    z-index: 999;
  }
  .themeTitle>div{
    float: left;
    width: calc(25% - 10px);
    text-align: center;
    height: 60px;
    line-height: 70px;
    font-size: 1.1rem;
    color: white;
    margin: 5px;
    box-sizing: border-box;

  }
  .loadRouter{
    margin-top: 80px;
  }
  .iconfont{
    font-size: 1.3rem;
  }
  .showBottomDiv{
    height: 10vh;
    width: 100%;
    float: left;
  }


</style>
