<template>
    <div class="table_contain">
        <!-- 表格 -->
        <el-table :data="tableData" style="width: 100%" :header-cell-style="{fontSize:'14px', color:'black'}">
            <!-- 列表内嵌列表 -->
            <el-table-column type="expand">
                <template slot-scope="props">
                    <el-form label-position="left" inline class="demo-table-expand">
                        <el-form-item label="店铺名称">
                            <span>{{ props.row.name }}</span>
                        </el-form-item>
                        <el-form-item label="店铺地址">
                        <span>{{ props.row.address }}</span>
                        </el-form-item>
                        <el-form-item label="店铺介绍">
                            <span>{{ props.row.description }}</span>
                        </el-form-item>
                        <el-form-item label="店铺 ID">
                            <span>{{ props.row.id }}</span>
                        </el-form-item>
                        <el-form-item label="联系电话">
                            <span>{{ props.row.phone }}</span>
                        </el-form-item>
                        <el-form-item label="评分">
                            <span>{{ props.row.rating }}</span>
                        </el-form-item>
                        <el-form-item label="销售量">
                            <span>{{ props.row.recent_order_num }}</span>
                        </el-form-item>
                        <el-form-item label="分类">
                            <span>{{ props.row.category }}</span>
                        </el-form-item>
                    </el-form>
                </template>
            </el-table-column>
            <!-- 表头 -->
            <el-table-column label="店铺名称" prop="name" </el-table-column>
            <el-table-column label="店铺地址" prop="address"></el-table-column>
            <el-table-column label="店铺介绍" prop="description"></el-table-column>
            <el-table-column label="操作" width="400" class="font">
                <template slot-scope="scope">
                    <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="mini" type="Success" @click="addFood(scope.$index, scope.row)">添加食品</el-button>
                    <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <div class="Pagination">
            <el-pagination @size-change="handleSizeChange" @curren-change="handleCurrentChange"
                :current-page="currentPage" :page-size="20"
                layout="total, prev, pager, next" :total="count">
            </el-pagination>
        </div>
        <!-- 修改店铺信息 -->
        <el-dialog title="修改店铺信息" v-model="dialogFormVisible">
            <!-- 对应的店铺 -->
            <el-form :model="selectTable">
                <el-form-item label="店铺名称" label-width="100px">
                    <el-input v-model="selectTable.name" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="详细地址" label-width="100px">
                    <el-autocomplete v-model="address.address" :fetch-suggestions="querySearchAsync"
                        placeholder="请输入地址" style="width: 100%;"@select="addressSelect">
                    </el-autocomplete>
                    <span>当前城市：{{city.name}}</span>
                </el-form-item>
                <el-form-item label="店铺介绍" label-width="100px">
                    <el-input v-model="selectTable.description"></el-input>
                </el-form-item>
                <el-form-item label="联系电话" label-width="100px">
                    <el-input v-model="selectTable.phone"></el-input>
                </el-form-item>
                <el-form-item label="店铺分类" label-width="100px">
                    <el-cascader :options="categoryOptions" v-model="selectedCategory" change-on-select>
                    </el-cascader>
                </el-form-item>
                    <el-form-item label="商铺图片" label-width="100px">
                        <el-upload class="avatar-uploader" :action="baseUrl + '/v1/addimg/shop'"
                            :show-file-list="false" :on-success="handleServiceAvatarScucess"
                            :before-upload="beforeAvatarUpload">
                            <img v-if="selectTable.image_path" :src="baseImgPath + selectTable.image_path" class="avatar">
                            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                        </el-upload>
                    </el-form-item>
            </el-form>
        </el-dialog>
    </div>
</template>
<script>
export default {
    name: 'shoplist',
    data(){
        return{
            baseUrl,
                baseImgPath,
                city: {},
                offset: 0,
                limit: 20,
                count: 0,
                tableData: [],
                currentPage: 1,
                selectTable: {},
                dialogFormVisible: false,
                categoryOptions: [],
                selectedCategory: [],
                address: {},
        }
    },
    created(){
        this.getshoplist();
    },
    methods:{
        async querySearchAsync(queryString, cb) {
                console.log(2)
        },
        addressSelect(vale){
            console.log(vale)
        },
        handleEdit(index, row){
            console.log(index,row)
        },
        addFood(index, row){
            console.log(index,row)

        },
        // 删除商家
        handleDelete(index, row){
            this.$confirm("确认删除？")
            .then(result =>{
                let id=this.tableData[index].id;
                var q=this.deleteResturnant(id);
                if(q == false){
                    this.$message({
                        type: 'success',
                        message: '删除店铺成功',
                    });
                    this.tableData.splice(index, 1) //通过下标删除页面上的数据
                }else{
                    alert("失败")
                }
            })
            .catch(err=>{console.log(err)})
        },
        // 获取商家列表 api
        getshoplist(){
            this.$http.get("http://localhost:3000/shoplist")
                .then(res=>{
                    // console.log(res.data);
                    this.tableData=res.data
                })
        },
        // 删除商家 api
        deleteResturnant(id){
            this.$axios({
                method:"delete",
                url:`http://localhost:3000/shoplist/${id}`,
            }).then(res=>{
                // console.log(res)
            }).catch(err=>{
                console.log(err)
            })
            return false;
        },
        // 分页
        handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
            this.currentPage = val;
            this.offset = (val - 1)*this.limit;
            this.getResturants()
        },
    }
}
</script>
<style scoped>
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
    .table_container{
        padding: 20px;
    }
    .Pagination{
        display: flex;
        justify-content: flex-start;
        margin-top: 8px;
    }
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
        border-color: #20a0ff;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 120px;
        height: 120px;
        line-height: 120px;
        text-align: center;
    }
    .avatar {
        width: 120px;
        height: 120px;
        display: block;
    }
</style>

