<template>
  <div id="agreeFriend">
    <prev-top title="新的朋友"></prev-top>
    <div @click="showSearch" class="search">
      <icon name="search" width="20" height="20"></icon><span>搜索</span>  
    </div>
    <div class="card-wrap">
        <van-swipe-cell v-for="(item, index) in friendInfo" :key="index" :right-width="65" >
          <van-cell-group>
            <!-- <van-cell title="单元格" value="内容" /> -->
            <div class="card">
                <div class="card-icon">
                  <img :src="item.friendavatar" alt="">
                </div>
                <div class="remark-wrap">
                  <p>{{item.requestnickname}}</p>
                  <p class="remark">{{item.requestremark}}</p>
                </div>
                <div class="selected" @click="getRequestFriend(item.requestname)" >查看</div>
            </div>
          </van-cell-group>
          <span slot="right">删除</span>
        </van-swipe-cell>
    </div>
    <popup :popupVisible="isShow" :closePopup="closePopup"></popup> 
    <router-view></router-view>
  </div>
</template>

<script>
import Vue from "vue";
import { SwipeCell, CellGroup, Cell } from "vant";
Vue.use(SwipeCell);
Vue.use(CellGroup);
Vue.use(Cell);

import PrevTop from "com/prevTop/prevTop";
import Popup from "com/popup/popup";
export default {
  data() {
    return {
      isShow: false,
      friendInfo: null
    };
  },
  components: {
    PrevTop,
    Popup
  },
  methods: {
    showSearch: function() {
      // this.$router.push('/search')
      this.isShow = true;
    },
    closePopup: function(isShow) {
      this.$set(this, "isShow", isShow);
    },
    getRequestFriend: function(requestname){
      this.$router.push({path:`/agreefriend/${requestname}`})
    }
  },
  mounted() {
    const _this = this;
    this.$http({
      method: "GET",
      url: "/api/getRequestFriend",
      params: {
        username: _this.$store.state.userInfo.username
      }
    }).then(res => {
      console.log(res);
      let data = res.data
      if(data.success){
        this.friendInfo = data.data
      }
    });
  }
};
</script>

<style lang="less" >
.search {
  width: 98%;
  margin: 5px auto;
  margin-top: 45px;
  background: #f0f0f0;
  border-radius: 3px;
  height: 25px;
  display: flex;
  justify-content: center;
  align-items: center;
  span {
    margin-left: 3px;
    font-size: 15px;
    line-height: 25px;
    color: #bfbfbf;
  }
}
 
 .card-wrap .van-swipe-cell__right {
    color: #ffffff;
    font-size: 15px;
    width: 65px;
    height: 80px;
    display: inline-block;
    text-align: center;
    line-height: 80px;
    background-color: #f44;
}
.card{
  height: 80px;
  display: flex;
  align-items: center;
  box-sizing: border-box;
  justify-content: space-between;
  padding: 0 20px;
  .card-icon{
    width: 60px;
    height: 60px;
    border-radius: 3px;
    img{
      width: 100%;
    }
  }
  .remark-wrap{
    flex:1;
    margin-left: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    p{
      margin: 0;
      font-size: 15px;
    }
    .remark{
      color: #999;
      font-size: 12px;
      margin-top: 5px;
    }
  }
}
.selected{
  padding: 5px 10px;
  background-color: #f0f0f0;
  border: 1px solid #d6d6d6;
  color:#000;
  border-radius: 3px;
  font-size: 12px;
}
</style>

