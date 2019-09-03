<template>
  <el-card class="material">
    <bread-crumb slot="header">
      <template slot="title">素材管理</template>
    </bread-crumb>
    <el-tabs v-model="activeName" @tab-click="changeTab">
      <el-tab-pane label="全部素材" name="all">
        <!-- 全部素材的内容 -->
        <div class="card-list">
          <el-card class="img-card" v-for="item in list" :key="item.id">
            <img :src="item.url" alt />
            <el-row class="operate" type="flex" align="middle" justify="space-around">
              <i :style="{color: item.is_collected ? 'red' : '' }" class="el-icon-star-on"></i>
              <i class="el-icon-delete-solid"></i>
            </el-row>
          </el-card>
        </div>
        <el-row type='flex' justify="center">
          <el-pagination @current-change="changePage" :current-page="page.page" :page-size="page.pageSize"  :total="page.total" background layout="prev, pager, next" ></el-pagination>
        </el-row>
      </el-tab-pane>
      <el-tab-pane label="收藏图片" name="collect">
        <!-- 收藏素材的内容 -->
        <div class="card-list">
          <el-card class="img-card" v-for="item in list" :key="item.id">
            <img :src="item.url" alt />
          </el-card>
        </div>
         <el-row type='flex' justify="center">
          <el-pagination @current-change="changePage" :current-page="page.page" :page-size="page.pageSize"  :total="page.total" background layout="prev, pager, next" ></el-pagination>
        </el-row>
      </el-tab-pane>
    </el-tabs>
  </el-card>
</template>

<script>
export default {
    data(){
        return{
            activeName : 'all',
            list:[],
            page:{
                page :1,
                pageSize: 10,
                total : 0
            }
        }
    },
    methods :{
        changePage(newPage){
            this.page.page = newPage
            this.getMaterial()
        },
    chageTab(){
        this.page.page = 1
        this.getMaterial()
    },
    getMaterial(){
        this.$axios({
            url : '/user/images',
            params :{
                page : this.page.page,
                per_page:this.page.pageSize,
                collect:this.activeName === 'collect'
            }
        }).then(result =>{
            this.list = result.data.results
            this.page.total = result.data.total_count
        })
      }
    },
    created(){
        this.getMaterial()
    }
}
</script>

<style lang="less" scoped>
.material {
  .card-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    .img-card {
      width: 180px;
      height: 180px;
      margin: 30px;
      position: relative;
      img {
        width: 100%;
        height: 100%;
      }
      .operate {
        position: absolute;
        width: 100%;
        bottom: 0;
        left: 0;
        height: 30px;
        background-color: #f4f5f6;
        font-size: 18px;
      }
    }
  }
}
</style>