<template>
    <div>
        <div class="wrapper6" >
            <el-col>
                <span class="lebal">项目名称</span>
                <el-input style="width:100%" v-model="name" placeholder="请输入内容"></el-input>
                <span class="lebal">项目编号</span>
                <el-input  style="width:100%" v-model="id" placeholder="请输入内容"></el-input>
                <span class="lebal">客户名称</span>
                <el-input style="width:100%" type="textarea" :rows="4" placeholder="请输入内容" v-model="cus_name"></el-input>
                <span class="lebal">要求系数</span><br>
                <el-select size="mini" v-model="req_coe" placeholder="请选择">
                    <el-option
                        v-for="item in options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"><!--data未设置 -->
                    </el-option>
                </el-select>
                <el-row style="float: right; top: -30px; left:18px">
                    <el-button circle size="small" @click="editReq">编辑</el-button><!--弹窗 未实现 -->
                    <el-button circle size="small" @click="dialogFormVisible = true">添加</el-button>
                    <el-dialog title="要求系数" :visible.sync="dialogFormVisible">
                        <el-form :model="form">
                            <el-form-item label="type name" :label-width="formLabelWidth">
                                <el-input v-model="typename" auto-complete="off"></el-input>
                            </el-form-item>
                            <el-form-item label="default units" :label-width="formLabelWidth">
                                <el-select v-model="units1" placeholder="请选择活动区域">
                                    <el-option label="区域一" value="shanghai"></el-option>
                                    <el-option label="区域二" value="beijing"></el-option>
                                </el-select>
                                <el-select v-model="units2" placeholder="请选择活动区域">
                                    <el-option label="区域一" value="shanghai"></el-option>
                                    <el-option label="区域二" value="beijing"></el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="DP Dimension" :label-width="formLabelWidth">
                                <el-select v-model="DP_Demision" placeholder="请选择活动区域">
                                    <el-option label="区域一" value="shanghai"></el-option>
                                    <el-option label="区域二" value="beijing"></el-option>
                                </el-select>
                            </el-form-item>
                        </el-form>
                        <div slot="footer" class="dialog-footer">
                            <el-button @click="dialogFormVisible = false">取 消</el-button>
                            <el-button type="primary" @click="saveReq_Coe">确 定</el-button>
                        </div>
                    </el-dialog>
                </el-row>
            </el-col>
        </div>
        <div class="wrapper6">
            <el-col><br>
                <el-switch style="position:relative;" v-model="value1" active-text="use demand value from floor" inactive-text=" ">></el-switch><br><br>
                <el-switch style="position:relative;" v-model="value2" active-text="use supplied data needed" inactive-text=" ">></el-switch><br><br>
                <span class="lebal">Directional</span><br>
                <el-radio v-model="choose1" label="1">备选项</el-radio>
                <el-radio v-model="choose1" label="2">备选项</el-radio><br>
                <span class="lebal">Correlation</span><br>
                <el-radio v-model="choose2" label="1">备选项</el-radio>
                <el-radio v-model="choose2" label="2">备选项</el-radio><br><br><br>
                <el-button style="display:block;margin:0 auto" @click="savegen">下一步</el-button>
            </el-col>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                dialogTableVisible: false,
                dialogFormVisible: false,
                form: {
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    delivery: false,
                    type: [],
                    resource: '',
                    desc: ''
                },
                typename: '',
                DP_Demision: '',
                units1: '',
                units2: '',
                formLabelWidth: '120px',
                value2: false,
                value1: false,
                choose1: '1',
                choose2: '1',
                id:"",
                name:"",
                describe:"",
                cus_name:"",
                options: [{
                    value: '选项1',
                    label: 'DB_Common'
                }, {
                    value: '选项2',
                    label: 'DB_School'
                }, {
                    value: '选项3',
                    label: 'DB_Hospital'
                }, {
                    value: '选项4',
                    label: 'DB_User'
                }, {
                    value: '选项5',
                    label: 'DB_Office'
                }, {
                    value: '选项6',
                    label: 'DB_FEMA'
                }],
                req_coe:'',
                val1: '11'
            }
        },

        mounted: function () {
            var vm = this
            // 用$on事件来接收参数
            var label = JSON.parse(localStorage.getItem("label"));
            //console.log(input+'!!!!!');
            //var tempdata = this.data[input];
            //this.newData[input] = tempdata;
            localStorage.removeItem("label");
    },


        methods: {
            saveReq_Coe(){
                this.dialogFormVisible = false;
                //post数据去后台提交新建请求或修改
            },

            editReq(){
                this.dialogFormVisible = true;
                //向后台请求数据改变data的值
            },

            savegen() {//保存当前页面内容
                var gen_info = {
                    name: this.name, 
                    id: this.id, 
                    req_coe: this.req_coe,
                    cus_name: this.cus_name,
                    value1: this.value1,
                    value2: this.value2,
                    choose1: this.choose1,
                    choose2: this.choose2
                };
                localStorage.setItem("gen_info",JSON.stringify(gen_info));
                this.$router.push({name:'notes'});
            },
            open() {
                this.$alert('这是一段内容', '要求系数编辑', {
                    confirmButtonText: '确定',
                    callback: action => {
                        this.$message({
                            type: 'info',
                            message: `action: ${ action }`
                        });
                    }
                });
            }
        }
    }
</script>


<style>
    .wrapper6{
        position:relative;/*相对定位:参考物*/
        float:left;/*浮动:左浮动 与父元素的左端对齐 依次的往右端显示 一行显示不下就换行接着依次显示*/
        top:-15px;
        width:43%;
        height:350px;
        margin:18px 20px;
      
    }

    .lebal{
        position:relative;
        display: inline-block;
        padding:12px 0;
        color: #333;
    } 

</style>