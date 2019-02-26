<template>
    <el-container style="height: 100%; ">
        <el-aside width="150px"  style=" height:938px;position: relative; background-color: rgb(66, 59, 63);">
            <div class="info">
                <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1550473548073&di=cb7382deb5e6f48702ed576c770df950&imgtype=0&src=http%3A%2F%2Fimg4.duitang.com%2Fuploads%2Fitem%2F201412%2F24%2F20141224224554_SuYth.thumb.700_0.jpeg"/>
                <p>雨桐的杂货店</p>
            </div>
            <el-menu default-active="1"
                     background-color="rgb(66, 59, 63)"
                     text-color="#FFF"
                     active-text-color="#ffd04b"
                     style="border:none;">
                <el-menu-item index="1" >
                    <i class="el-icon-document"></i>
                    <span slot="title">客户信息</span>
                </el-menu-item>
                <el-menu-item index="2">
                    <i class="el-icon-menu"></i>
                    <span slot="title">日常业务</span>
                </el-menu-item>
                <el-menu-item index="3">
                    <i class="el-icon-search"></i>
                    <span slot="title">库存查询</span>
                </el-menu-item>
                <el-menu-item index="4">
                    <i class="el-icon-tickets"></i>
                    <span slot="title">财务报表</span>
                </el-menu-item>
            </el-menu>
            <div class="exit">
                <el-button type="text" icon="el-icon-d-arrow-right">退出</el-button>
            </div>
        </el-aside>

        <el-container>
            <el-main>
                <el-tabs v-model="information" @tab-click="handleClick">
                    <el-tab-pane label="客户信息" name="user_info">
                        <el-container>
                            <el-header style="line-height: 60px">
                                <div class="search_form">
                                    <el-input placeholder="请输入内容"v-model="user_search_key">
                                        <el-button slot="append" icon="el-icon-search">搜索</el-button>
                                    </el-input>
                                </div>
                                <el-button type="primary" style="margin-left:20px;">新增</el-button>
                            </el-header>
                            <el-main>
                                <el-table
                                        stripe:true
                                        border:true
                                        highlight-current-row:true
                                        :data="user_info"
                                        style="width: 100%">
                                    <el-table-column type="expand">
                                        <template slot-scope="props">
                                            <el-form label-position="left" inline class="table-expand">
                                                <el-form-item label="姓 名">
                                                    <span>{{ props.row.name }}</span>
                                                </el-form-item>
                                                <el-form-item label="性 别">
                                                    <span>{{ props.row.sex }}</span>
                                                </el-form-item>
                                                <el-form-item label="手机号">
                                                    <span>{{ props.row.phone }}</span>
                                                </el-form-item>
                                                <el-form-item label="地 址">
                                                    <span>{{ props.row.address }}</span>
                                                </el-form-item>
                                                <el-form-item label="商品分类">
                                                    <span>{{ props.row.category }}</span>
                                                </el-form-item>
                                                <el-form-item label="店铺地址">
                                                    <span>{{ props.row.address }}</span>
                                                </el-form-item>
                                                <el-form-item label="商品描述">
                                                    <span>{{ props.row.desc }}</span>
                                                </el-form-item>
                                            </el-form>
                                        </template>
                                    </el-table-column>
                                    <el-table-column
                                            type="index"
                                            :index="itemIndexMethod">
                                    </el-table-column>
                                    <el-table-column
                                            label="姓名"
                                            prop="name">
                                    </el-table-column>
                                    <el-table-column
                                            label="性别"
                                            prop="sex">
                                    </el-table-column>
                                    <el-table-column
                                            label="手机号"
                                            prop="phone">
                                    </el-table-column>
                                    <el-table-column
                                            label="地址"
                                            prop="address">
                                    </el-table-column>
                                    <el-table-column label="操作">
                                        <template slot-scope="scope">
                                            <el-button @click="showEditUser(scope.$index, scope.row)" size="small" type="primary" icon="el-icon-edit" circle></el-button>
                                            <el-button @click="deleteUser(scope.$index, scope.row)" size="small" type="danger" icon="el-icon-delete" circle></el-button>
                                        </template>
                                    </el-table-column>
                                </el-table>
                            </el-main>
                            <!-- 修改用户信息页面 -->
                            <el-dialog
                                    title="修改用户信息"
                                    :visible.sync="editUserFormVisible"
                                    width="500px"
                                    >
                                <el-form :model="current_edit_user" :rules="user_rules" ref="current_edit_user" label-width="100px" class="">
                                    <el-form-item label="姓名" prop="name">
                                        <el-input v-model="current_edit_user.name" placeholder="请输入用户姓名"></el-input>
                                    </el-form-item>
                                    <el-form-item label="手机号" prop="phone">
                                        <el-input v-model="current_edit_user.phone" placeholder="请输入用户的手机号"></el-input>
                                    </el-form-item>
                                    <el-form-item label="QQ号" prop="qq">
                                        <el-input v-model="current_edit_user.qq" placeholder="请输入用户的 QQ 号"></el-input>
                                    </el-form-item>
                                    <el-form-item label="微信" prop="wechat">
                                        <el-input v-model="current_edit_user.wechat" placeholder="请输入用户的微信号"></el-input>
                                    </el-form-item>
                                    <el-form-item label="性别" prop="sex">
                                        <el-radio v-model="current_edit_user.sex" label="0" >男</el-radio>
                                        <el-radio v-model="current_edit_user.sex" label="1" >女</el-radio>
                                    </el-form-item>
                                    <el-form-item label="地址" prop="address">
                                        <el-input v-model="current_edit_user.address" placeholder="请输入用户的家庭住址"></el-input>
                                    </el-form-item>
                                    <el-form-item label="备注" prop="desc">
                                        <el-input type="textarea" autosize v-model="current_edit_user.desc" placeholder="请输入备注信息"></el-input>
                                    </el-form-item>
                                </el-form>
                                <div slot="footer" class="dialog-footer">
                                    <el-button @click="editUserFormVisible = false;">取 消</el-button>
                                    <el-button type="primary" @click="editUser('current_edit_user')">确 定</el-button>
                                </div>
                            </el-dialog>
                        </el-container>
                    </el-tab-pane>

                    <el-tab-pane label="宠物信息" name="pet_info">
                        <el-container>
                            <el-header style="line-height: 60px">
                                <div class="search_form">
                                    <el-input placeholder="请输入内容"v-model="pet_search_key">
                                        <el-button slot="append" icon="el-icon-search">搜索</el-button>
                                    </el-input>
                                </div>
                                <el-button type="primary" style="margin-left:20px;">新增</el-button>
                            </el-header>
                            <el-main>
                                <el-table
                                        stripe:true
                                        border:true
                                        highlight-current-row:true
                                        :data="pet_info"
                                        style="width: 100%">
                                    <el-table-column type="expand">
                                        <template slot-scope="props">
                                            <el-form label-position="left" inline class="table-expand">
                                                <el-form-item label="姓 名">
                                                    <span>{{ props.row.name }}</span>
                                                </el-form-item>
                                                <el-form-item label="性 别">
                                                    <span>{{ props.row.shop }}</span>
                                                </el-form-item>
                                                <el-form-item label="手机号">
                                                    <span>{{ props.row.id }}</span>
                                                </el-form-item>
                                                <el-form-item label="地 址">
                                                    <span>{{ props.row.shopId }}</span>
                                                </el-form-item>
                                                <el-form-item label="商品分类">
                                                    <span>{{ props.row.category }}</span>
                                                </el-form-item>
                                                <el-form-item label="店铺地址">
                                                    <span>{{ props.row.address }}</span>
                                                </el-form-item>
                                                <el-form-item label="商品描述">
                                                    <span>{{ props.row.desc }}</span>
                                                </el-form-item>
                                            </el-form>
                                        </template>
                                    </el-table-column>
                                    <el-table-column
                                            type="index"
                                            :index="itemIndexMethod">
                                    </el-table-column>
                                    <el-table-column
                                            label="姓名"
                                            prop="name">
                                    </el-table-column>
                                    <el-table-column
                                            label="宠物名"
                                            prop="pet_name">
                                    </el-table-column>
                                    <el-table-column
                                            label="品种"
                                            prop="desc">
                                    </el-table-column>
                                    <el-table-column
                                            label="年龄"
                                            prop="age">
                                    </el-table-column>
                                    <el-table-column
                                            label="日期"
                                            prop="date">
                                    </el-table-column>
                                    <el-table-column label="操作">
                                        <template slot-scope="scope">
                                            <el-button @click="editPet(scope.$index, scope.row)" size="small" type="primary" icon="el-icon-edit" circle></el-button>
                                            <el-button @click="deletePet(scope.$index, scope.row)" size="small" type="danger" icon="el-icon-delete" circle></el-button>
                                        </template>
                                    </el-table-column>
                                </el-table>
                            </el-main>
                        </el-container>
                    </el-tab-pane>
                </el-tabs>
            </el-main>
        </el-container>
    </el-container>
</template>

<style>
    .info {
        height: 150px;
        margin: 30px 0;
        text-align:center;
    }
    .info>img{
        width:100px;
        heigth:100px;
        border-radius: 50%;
    }
    .info>p{
        color:#FFF;
        font-size:14px;
    }
    .exit{
        width: 100%;
        text-align: center;
        border-top: 1px #fff solid;
        position: absolute;
        bottom: 0px;
    }
    .exit>button{
        color:#fff;
    }
    .search_form{
        width: 300px;
        display: inline-block;
    }
    .table-expand {
        font-size: 0;
    }
    .table-expand label {
        width: 90px;
        color: #99a9bf;
    }
    .table-expand .el-form-item {
        margin-right: 0;
        margin-bottom: 0;
        width: 50%;
    }
</style>

<script>
    export default {
        data() {
            return {
                information: 'user_info',           //默认显示客户信息
                user_search_key:"",                 //用户搜索关键字
                pet_search_key:"",                  //宠物搜索关键字
                current_edit_user: {                //当前编辑的用户信息
                    id:'',
                    name: '',
                    phone: '',
                    qq: '',
                    wechat: '',
                    sex: [],
                    address: '',
                    desc: ''
                },
                editUserFormVisible: false,         //编辑用户信息页面开关
                editPetFormVisible: false,          //编辑宠物信息页面开关
                user_info: [{
                    id: '12987122',
                    name: '好滋好味鸡蛋仔',
                    phone:'15838844213',
                    sex:'男',
                    category: '江浙小吃、小吃零食',
                    desc: '荷兰优质淡奶，奶香浓而不腻',
                    address: '上海市普陀区真北路',
                    shop: '王小虎夫妻店',
                    shopId: '10333'
                }],
                pet_info: [{
                    id: '12987122',
                    name: '好滋好味鸡蛋仔',
                    pet_name:'毛毛',
                    age:2.4,
                    date:'2018-2-5',
                    category: '江浙小吃、小吃零食',
                    desc: '荷兰优质淡奶，奶香浓而不腻',
                    address: '上海市普陀区真北路',
                    shop: '王小虎夫妻店',
                    shopId: '10333'
                }],
                user_rules: {
                    name: [
                        {required: true, message: '请输入用户名称', trigger: 'blur'},
                        {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
                    ],
                    phone: [
                        {required: true, message: '请输入用户的手机号', trigger: 'blur'},
                        {min: 6, max: 11, message: '长度在 6 到 11 个字符', trigger: 'blur'}
                    ],
                    qq: [
                        {min: 6, max: 11, message: '长度在 6 到 11 个数字', trigger: 'blur'}
                    ],
                    wechat: [
                        { required: true, message: '请输入用户的微信号', trigger: 'blur'}
                    ],
                    sex: [
                        { required: true, message: '请选择用户的性别', trigger: 'blur'}
                    ],
                    address: [
                        {required: true, message: '请输入用户的地址', trigger: 'blur'}
                    ],
                    desc: [
                        { message: '请填写活动形式', trigger: 'blur'}
                    ]
                },
            };


        },
        methods: {
            handleClick(tab, event) {
                console.log(tab, event);
            },
            itemIndexMethod(index) {            //自动表格生成序号
                return index;
            },
            addUser(){                          //添加用户

            },
            showEditUser(index, row){                     //显示修改用户信息页面
                this.resetForm('current_edit_user');      //重置用户信息页面信息
                //获取信息
                this.editUserFormVisible = true;
            },
            editUser(formName) {                        //修改用户信息
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
                // this.editUserFormVisible = false;
            },
            deleteUser() {                      //删除用户

            },
            addPet(){                           //添加宠物

            },
            showEditPet(){                      //显示修改宠物信息界面

            },
            editPet(index, row) {               //修改宠物信息
                console.log(row);
            },
            deletePet() {                       //删除宠物

            },
            resetForm(formName) {               //重置表格信息
                this.$nextTick(()=> {           //用于延迟执行一段代码,等待
                    this.$refs[formName].resetFields();         //重置表格的信息
                });
            }
        },
    }
</script>

