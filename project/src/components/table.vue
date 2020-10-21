<template>
  <div class="teble">
    <el-table :data="tableData"
              v-loading="load"
              stripe
              >
      <el-table-column v-for="(value,index) in tableColumns"
                       :key="index"
                       :prop="value.prop"
                       :align="value.align"
                       :label="value.label"
                       :width="value.width"></el-table-column>
    </el-table>

    <el-pagination @size-change="handleSizeChange"
                   @current-change="handleCurrentChange"
                   :current-page="currentPage"
                   :page-sizes="[5, 10, 15, 20]"
                   :page-size="pageSize"
                   background
                   layout="total, sizes, prev, pager, next, jumper"
                   align="center"
                   :total="this.totalNum"></el-pagination>
  </div>
</template>

<script>
    import axios from "axios"

    export default {
        data() {
            return {
                currentPage: 1,
                pageSize: 10,
                pageNum: 1,
                totalNum: 0,
                load: false,
                tableData: [],
                tableColumns: [{
                    prop: "setname",
                    label: "设备名称",
                    align: "center",
                    width: ""
                }, {
                    prop: "typecode",
                    label: "设备类型编码",
                    align: "center",
                    width: ""
                }, {
                    prop: "creator",
                    label: "创建人",
                    align: "center",
                    width: ""
                }, {
                    prop: "createtime",
                    label: "创建时间",
                    align: "center",
                    width: ""
                }]
            };
        },

        methods: {
            loadData(reset) {
                var params = {
                    _page: this.pageNum,
                    _limit: this.pageSize
                }
                this.load = true;
                axios({
                    type: "get",
                    url: "/api/sets",
                    params
                }).then(res => {
                    var data = res.data;
                    if (data.errno == 0) {
                        this.tableData = data.data;
                        this.totalNum = (this.tableData.length-1)*this.pageSize;
                    } else {
                        console.log(data)
                    }
                }).finally(res => {
                    this.load = false
                })


            },
            handleSizeChange(val) {
                this.pageSize = val;
                this.loadData()
            },
            handleCurrentChange(val) {
                this.pageNum = val;
                this.loadData()
            },
        },
        mounted() {
            this.loadData()
        },
    };
</script>

<style>

</style>