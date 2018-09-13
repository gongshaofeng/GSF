<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 项目管理</el-breadcrumb-item>
            </el-breadcrumb>
            <div class="handle-box">
                <el-input size="medium"  placeholder="请输入项目名称" style="width:300px" class="handle-input mr10"></el-input>
                <el-button size="medium" type="primary" icon="el-icon-search" class="box-serach">搜索</el-button>
                <el-button type="primary" size="medium" icon="el-icon-plus">添加</el-button>
            </div>
        </div>
        <el-table :data="tableData" style="width: 100% ;padding:0 24px; margin-top: 10px;" >
            <el-table-column type="index" label="序号" width="50"></el-table-column>
            <el-table-column label="项目编号" prop="pid" ></el-table-column>
            <el-table-column  label="项目名称" prop="pname" ></el-table-column>
            <el-table-column label="创建时间" prop="addtime" ></el-table-column>
            <el-table-column label="操作">
                <template slot-scope="scope">
                    <el-button
                        size="mini"
                        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button
                        size="mini"
                        type="danger"
                        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="block">
                <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange"
                                :current-page.sync="currentPage3" :page-size="100"
                                layout="prev, pager, next, jumper" :total="500" class="pagination">
                </el-pagination>
        </div>
    </div>
</template>
<script>
export default {
    name:'basetable',
    data() {
        return {
            // tableData: [{
            // pid: "sf-001",
            // pname: '一二三四',
            // addtime: '2016-05-02',
            // }, {
            // pid: "sf-002",
            // pname: '一二三四五',
            // addtime: '2016-05-02',
            // }, {
            // pid: "sf-003",
            // pname: '一二三四五六',
            // addtime: '2016-05-02',
            // }, {
            // pid: "sf-004",
            // pname: '一二三四五六七',
            // addtime: '2016-05-02',
            // }]
            tableData:[],
            currentPage:5
        }
    },
    created(){
        // 获取项目列表
        this.getProlist();
    },
    methods: {
        handleEdit(index, row) {
            console.log(index, row);
        },
        handleDelete(index, row) {
            console.log(index, row);
        },
        getProlist(){
            this.$http.get("http://localhost:3000/project")
                .then(result=>{
                    if(result){
                        this.tableData=result.data;
                        // console.log(this.tableData)
                    }else{
                        alert("失败")
                    }
                })
        }
    }
}


</script>
<style scoped>
.table {
    padding: 20px;
}
.box-serach {
    margin-left: 10px;
}
.handle-box{
    margin-top: 20px;
}
.handle-select{
    width: 150px;
}
.handle-input{
    width: 150px;
    display: inline-block;
}
.pagination{
    display: flex;
    justify-content: center;
    margin-top: 50px;
}
</style>
