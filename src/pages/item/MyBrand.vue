<template>
    <div>
        <v-layout class="px-3 pb-2">              <!--控制一行；px-3 pb-2调整横纵坐标的情况，css样式不用写，通过属性就行-->
            <v-flex xs2>                          <!-- v-flex可以通过xs控制宽度 -->
                <v-btn small color="info">新增品牌</v-btn>
            </v-flex>
            <v-spacer/>                           <!--专门负责把空间撑开，不要靠太近了-->
            <v-flex xs4>
                <v-text-field label="搜索" hide-details v-model="key"></v-text-field>    <!-- hide-details是填错的时候会提示错误信息，把它忽略，就不会预留底下的空间 -->
            </v-flex>
        </v-layout>
        <v-data-table :headers="headers"
                      :items="brands"
                      :pagination.sync="pagination"
                      :total-items="totalBrands"
                      :loading="loading"
                      class="elevation-1">
              <template slot="items" slot-scope="props">                   <!--自动遍历items-->
                <td class="text-xs-center">{{ props.item.id }}</td>
                <td class="text-xs-center">{{ props.item.name }}</td>
                <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
                <td class="text-xs-center">{{ props.item.letter }}</td>
                <td class="text-xs-center">
                    <v-btn flat icon color="info">                        <!--图标按钮控件-->
                        <v-icon>edit</v-icon>
                    </v-btn>
                    <v-btn flat icon color="error">
                        <v-icon>delete</v-icon>
                    </v-btn>
                </td>
              </template>
        </v-data-table>
    </div>
</template>

<script>
  export default {
      data () {
        return {
          headers: [            // 表头，Vuetify的API可以查
            { text: '品牌ID', value: 'id', align: 'center', sortable: true},
            { text: '品牌名称', value: 'name', align: 'center', sortable: false },     // 显示字段名、数据库字段名、居中显示、是否排序
            { text: '品牌LOGO', value: 'image', align: 'center', sortable: false },
            { text: '品牌首字母', value: 'letter', align: 'center', sortable: true },
            { text: '操作', value: 'letter', align: 'center', sortable: true }
          ],
          brands: [],           // 品牌       表格主题数据，需要从后端获取
          totalBrands: 0,       // 品牌总数
          loading: false,      // 显示进度条，Vuetify的API可以查
          pagination: {},       // 分页相关
          key: ""               // 搜索条件
        }
      },
      // 将我们写好的带有vue变量的页面编译成HTML页面的过程叫mounted挂载
      mounted () {

      },
      // 页面一创造就加载created
      created() {
          /*
          this.brands = [
              {id:2032, name:"OPPO", image:"1.jpg", letter:"O"},      // 制造假数据，一般都是ajax从后台获取
              {id:2033, name:"飞利浦", image:"2.jpg", letter:"F"},
              {id:2034, name:"华为", image:"3.jpg", letter:"H"},
              {id:2036, name:"酷派", image:"4.jpg", letter:"K"},
              {id:2036, name:"魅族", image:"5.jpg", letter:"M"}
          ];
          this.totalBrands = 15 ;
          */

          // 去后台查询
          this.loadBrands();
      },
      // 数据一旦改变，页面自动渲染。这里可自定义监控data里的数据
      watch: {
          pagination: {        // 监控data里的pagination属性
              deep: true,       // 深度监控，复杂类型嘛
              handler () {      // 监控到之后如何处理
                this.loadBrands();
              }
          },
          key(){              // 监控查询条件key
              this.pagination.page = 1;       // 搜索之后就到第一页
              //this.loadBrands();            // 上面到第一页，监控触发，这句就可以省了
          }
      },
      methods: {
          loadBrands(){
              this.loading = true;
              this.$http.get("/item/brand/page", {
                  params: {
                      page: this.pagination.page,         //当前页     这些分页的值都是插件帮我们自动填充的
                      rows: this.pagination.rowsPerPage,  //每页大小
                      sortBy: this.pagination.sortBy,     //排序字段
                      desc: this.pagination.descending,   //是否降序
                      key: this.key                       //搜索条件
                  }
              }).then( resp=>{
                  console.log( resp )
                  this.brands = resp.data.items;
                  this.totalBrands = resp.data.total;
                  this.loading = false;
              })
          }
      }
  }
</script>

<style scoped>

</style>
