<template>
    <div>
        <el-table
                :data="tableData"
                border
               >
            <el-table-column
                    align = "center"
                    fixed
                    prop="id"
                    label="编号"
                    width="150">
            </el-table-column>
            <el-table-column
            align = "center"
                    prop="name"
                    label="图书名"
                    width="150">
            </el-table-column>
            <el-table-column
            align = "center"
                    prop="author"
                    label="作者"
                    width="150">
            </el-table-column>
             <el-table-column
            align = "center"
                    prop="publish"
                    label="出版社"
                    width="150">
            </el-table-column>
             <el-table-column
            align = "center"
                    prop="price"
                    label="价格"
                    width="150">
            </el-table-column>
            <el-table-column
            align = "center"
                    label="操作"
                   >
                <template slot-scope="scope">
                    <el-button @click="edit(scope.row)" type="primary" size="mini">修改</el-button>
                    <el-button @click="deleteBook(scope.row)" type="primary" size="mini">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-pagination
                background
                layout="prev, pager, next"
                :page-size="pageSize"
                :total="total"
                @current-change="page">
        </el-pagination>
    </div>
</template>

<script>
    export default {
        methods: {
            deleteBook(row){
                const _this = this
                axios.delete('http://localhost:8181/book/deleteById/'+row.id).then(function(resp){
                    _this.$alert('《'+row.name+'》删除成功！', '消息', {
                        confirmButtonText: '确定',
                        callback: action => {
                            window.location.reload()
                        }
                    })
                })
            },
            edit(row) {
                this.$router.push({
                    path: '/update',
                    query:{
                        id:row.id
                    }
                })
            },
            page(currentPage){
                const _this = this
                axios.get('http://localhost:8181/book/findAll/'+(currentPage-1)+'/6').then(function(resp){
                    console.log(resp)
                    _this.tableData = resp.data.content
                    _this.pageSize = resp.data.size
                    _this.total = resp.data.totalElements
                })
            }
        },

        data() {
            return {
                pageSize:'1',
                total:'11',
                tableData: [{
                    id: 1,
                    name: '解忧杂货店',
                    author: '东野圭吾',
                    publish: '作家出版社',
                    price: 15.4
                }, {
                    id: 2,
                    name: '追风筝的人',
                    author: '卡勒德·胡赛尼',
                    publish: '清华出版社',
                    price: 30.5
                }, {
                    id: 3,
                    name: '人间失格',
                    author: '太宰治',
                    publish: '铁道出版社',
                    price: 30.6
                }]
            }
        },

        created() {
            const _this = this
            axios.get('http://localhost:8181/book/findAll/0/6').then(function(resp){
                console.log(resp)
                _this.tableData = resp.data.content
                _this.pageSize = resp.data.size
                _this.total = resp.data.totalElements
            })
        }
    }
</script>