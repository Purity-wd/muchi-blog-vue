<template>
  <div class="mcontaner">
    <Header></Header>

    <div class="block">
      <el-timeline>

        <el-timeline-item :timestamp="blog.createTime" placement="top" v-for="(blog,i) in blogs" :key='i'>
          <el-card>
            <h4>
              <router-link :to="{name: 'BlogDetail', params: {blogId: blog.id}}">
                {{blog.title}}
              </router-link>
            </h4>
            <p>{{blog.description}}</p>
          </el-card>
        </el-timeline-item>

      </el-timeline>

      <el-pagination class="mpage"
                     background
                     layout="prev, pager, next"
                     :current-page="currentPage"
                     :page-size="pageSize"
                     :total="total"
                     @current-change=page>
      </el-pagination>

    </div>

  </div>
</template>

<script>
  import Header from "../components/Header";

  export default {
    name: "Blogs.vue",
    components: {Header},
    data() {
      return {
        blogs: [{id:null,}],
        currentPage: 1,
        total: 0,
        pageSize: 5
      }
    },
    methods: {
      page(currentPage) {
        const _this = this
        _this.$axios.post("http://121.36.1.210:10580/api/blog/auth/page" ,{pageIndex:currentPage,pageSize:15}).then(res => {
          console.log('res',res)
          _this.blogs = res.data.data.rows
          // _this.currentPage = res.data.data.current
          // _this.total = res.data.data.total
          // _this.pageSize = res.data.data.size
          console.log(this.blogs)
        })
      }
    },
    created() {
      this.page(1)
    }
  }
</script>

<style scoped>
  .mpage {
    margin: 0 auto;
    text-align: center;
  }
</style>