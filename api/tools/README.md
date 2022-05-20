## 接口说明

> 提供一些接口给大家使用，免费，资源有限，暂时不限流。



<!DOCTYPE html>
<html>
<head>
    <meta charset=utf-8>
    <meta name=viewport content="width=device-width,initial-scale=1">
    <title>EveryDoc</title>
    <link rel="shortcut icon" href=/favicon.ico>
    <!-- 引入样式 -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <style>

        h1, h2 {
            font-weight: normal;
        }
    
        ul {
            list-style-type: none;
            padding: 0;
        }
    
        li {
            display: inline-block;
            margin: 0 10px;
        }
    
        a {
            color: #42b983;
        }
    
        .demo-table-expand {
            font-size: 0;
        }
        .demo-table-expand label {
            width: 90px;
            color: #99a9bf;
        }
        .demo-table-expand .el-form-item {
            margin-right: 0;
            margin-bottom: 0;
            width: 50%;
        }
    
    </style>
</head>
<body>
<div id="app">
    <el-container>
        <el-main>
            <el-row type="flex" justify="center">
                <el-col :xs="24" :sm="24" :md="18" :lg="16" :xl="8">
                    <h1>EveryDoc</h1>
                    <el-form :model="searchForm" @submit.native.prevent>
                        <el-form-item>
                            <el-input placeholder="You know for search" v-model="searchForm.keyword" class="input-with-select" style="max-width: 600px;" @keyup.enter.native="onSubmit">
                                <el-button slot="append" type="info" @click="onSubmit" icon="el-icon-search"></el-button>
                            </el-input>
                        </el-form-item>
                    </el-form>
                    <div class="grid-content bg-purple">
                        <div class="block">
                            <el-pagination layout="total, prev, pager, next"
                                           :total="this.total"
                                           :page-size="this.pageSize"
                                           :current-page="this.pageNum"
                                           :hide-on-single-page="true"
                                           @current-change="currentChange"
                            ></el-pagination>
                        </div>
                        <el-table v-loading="loading" :data="tableData" stripe style="width: 100%">
                            <template slot="empty">
                                <span style="color: #969799;">Search what you want</span>
                            </template>
                            <el-table-column type="expand" show-overflow-tooltip>
                                <template slot-scope="props">
                                    <el-form label-position="left" class="demo-table-expand">
                                        <el-form-item label="关键字:">
                                            <span>{{ props.row.keyword }}</span>
                                        </el-form-item>
                                        <el-form-item label="收录日期:">
                                            <span>{{ props.row.create_time }}</span>
                                        </el-form-item>
                                    </el-form>
                                </template>
                            </el-table-column>
                            <el-table-column prop="title" show-overflow-tooltip>
                                <template #default="scope">
                                    <a :href="scope.row.url" target="_blank">{{scope.row.title}}</a>
                                </template>
                            </el-table-column>
                        </el-table>
                    </div>
                </el-col>
            </el-row>
        </el-main>
    </el-container>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios@0.24.0/dist/axios.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data () {
            return {
                title: 'EveryDoc',
                tip: '提示信息',
                searchForm: {
                    keyword: '',
                    ext: 'pdf'
                },
                loading: false,
                total: 0,
                pageNum: 1,
                pageSize: 10,
                tableData: []
            }
        },
        methods: {
            onSubmit () {
                this.pageNum = 1
                this.search()
            },
            search () {
                this.loading = true
                if (this.searchForm.keyword == null || this.searchForm.keyword.trim() === '') {
                    return
                }
                let req = {
                    keyword: this.searchForm.keyword,
                    ext: this.searchForm.ext,
                    pageNum: this.pageNum,
                    pageSize: this.pageSize
                }
                axios.get('/tfsc/search', {params:req})
                    .then(res => {
                        if (res.data.data.list) {
                            this.tableData = res.data.data.list
                            this.total = res.data.data.total
                        } else {
                            this.$notify.info({
                                title: '系统消息',
                                message: '数据收集失败！！！'
                            })
                        }
                        this.loading = false
                    }).catch(err => {
                        this.loading = false
                    console.log(err)
                })
            },
            currentChange (page) {
                this.pageNum = page
                this.search()
            }
        }
    })
</script>
</body>
</html>

<script>
  new Vue({
    el: '#sidebar',
    data: {
        api: '关键字统计'
    },
    created() {
        fetch('https://pdf.imjcker.com/sys/menu')
          .then(response => response.json())
          .then(result => {
            document.getElementById('sidebar').innerHTML = marked.parse(result.data);
          })
          .catch(err => console.log(err));
      }
  })
</script>

