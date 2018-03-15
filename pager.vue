<template>
  <div id="page">
    <button class="btn btn-pager" :disabled="this.currentPager == 1"  @click=" pageto(1) "> 首页 </button>
    <span class="prePage" @click="prePager"> << </span>
    <span v-if="preClipped" class="page-index">...</span>

    <!-- 根据总页数创建分页标签 -->
    <span v-for="(item,index) in pagerArr" :key="item.index" 
          :class="{ active : item == currentPager }" 
          class="page-index" 
          @click="pageto( item )"
          >{{ item }}
    </span>
    <span v-if="nextClipped" class="page-index">...</span>
    <span class="nextPage" @click="nextPager"> >> </span>
    <button class="btn btn-pager" :disabled=" this.currentPager == this.totalPager" @click=" pageto(totalPager) "> 末页 </button>
  </div>
</template>

<style  lang="scss" scoped>
@import '../../assets/sass/function.scss';
#page{
  padding: px2emW(10px);
  font-size: 12px;
  width: 100%;
  text-align:center;
  color:#3727cf;
  position: absolute;
  bottom: px2emW(-10px);
  left: 0;
}
button{
  outline: none;
  border: none;
  color: #1e0c6e ;
  background-color: #fff;
}
.page-index{
  display: inline-block;
  height: px2emW(51px);
  width:  px2emW(51px);
  line-height: px2emW(51px);
  border-radius: 50%;
  background-color: #fff;
  text-align: center;
  margin: 0 px2emW(10px);
}
.active{
  background-color: #3727cf;
  color: #fff;
  margin:0 px2emW(15px);
}

</style>


<script>
export default {
  name : "Pager",
  props:['totalPager'],
  data () {
    return {
      preClipped :0,
      nextClipped : 0,
      currentPager : 1, // 当前页默认1
      limitPager : 5, // 分页最多显示
    }
  },
  methods : {
    // 跳转到点击页
    pageto : function (i){
      if ( i !== this.currentPager ){
        this.currentPager = i 
      }
    },
    // 跳到上一页
    prePager : function () {
      if( this.currentPager == 1 ) return
      this.currentPager--
    },
    // 跳到下一页
    nextPager : function () {
      if( this.currentPager == this.totalPager ) return
      this.currentPager ++
    },
  },
  computed : {
    // 分页数组
    pagerArr : function () {
      var left = 1 
      var right = this.totalPager
      var pagerArr = []
      this.nextClipped = 0
      this.preClipped = 0
      // 总页数大于5
      if(this.totalPager > this.limitPager){
         this.nextClipped = 1
         if(this.currentPager > this.limitPager - 2 && this.currentPager < this.totalPager - 2){
          this.nextClipped = 1
          this.preClipped = 1
          left = this.currentPager - 2
          right = this.currentPager + 2 
         }
         else{
            var n = this.totalPager - this.currentPager
            if( this.currentPager <= 3 ){
                this.preClipped = 0
                left = 1
                right = this.limitPager
            }
            else if (n <= 4){
                this.nextClipped = 0
                this.preClipped = 1
                right = this.totalPager
                left = this.totalPager - 4               
            }
            // 并不聪明的做法
            // else if(this.currentPager == this.totalPager - 2){
            //     this.nextClipped = 0
            //     this.preClipped = 1
            //     right = this.currentPager + 2
            //     left = this.currentPager - 2
            // }
            // else if(this.currentPager == this.totalPager ){
            //   this.preClipped = 1
            //   this.nextClipped = 0
            //   left = this.currentPager - 4
            //   right = this.currentPager 
            // }else if(this.currentPager == this.totalPager - 1){
            //   this.preClipped = 1
            //   this.nextClipped = 0
            //   right = this.currentPager + 1
            //   left = this.currentPager - 3
            // }
         }
      }
      while (left <= right){
        pagerArr.push(left)
        left ++
      }
      this.$emit('currentPager',this.currentPager)
      return pagerArr
    },
  },
  watch : {
    totalPager : function (val){
      this.currentPager = 1
    }
  }
}
</script>
