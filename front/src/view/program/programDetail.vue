<template>
    <div>
        <navi></navi>
        <div style="margin-top: 30px">
            <el-container>
                <el-aside width="200px">
                    <p class="content" ><span style="font-weight: bold">类型:</span>{{type}}<br/></p>
                    <p class="content" ><span style="font-weight: bold">领域:</span>{{area}}<br/></p>
                    <p class="content" ><span style="font-weight: bold">单位:</span>{{company}}<br/></p>
                    <p class="content" > <span style="font-weight: bold">描述:</span>{{keyword}}<br/></p>
                    <p class="content" > <span style="font-weight: bold">时间：</span>{{time}}<br/></p>
                    <p class="content" ><span style="font-weight: bold">所需人数：</span>{{number}}<br/></p>
                    <p class="content" ><span style="font-weight: bold">状态：</span>{{state}}<br/></p>
<!--                    <el-row style="margin: 10px"><el-button type="primary" round @click="edit">修改信息</el-button></el-row>-->
                </el-aside>
                <el-main>
                    <el-table :data="tableData"
                              stripe
                              style="width: 100%">
                        <el-table-column>
                            <!--<template slot="header"  scope="scope" >-->
                            <!--<el-input v-model="search" style="width: 300px"-->
                            <!--placeholder="请输入书名" prefix-icon="el-icon-search"/>-->
                            <!--</template>-->
                            <el-table-column
                                    prop="id"
                                    label="ID"
                            >
                            </el-table-column>
                            <el-table-column
                                    prop="expertID"
                                    label="专家编号"
                            ></el-table-column>
                            <el-table-column
                                    prop="time"
                                    label="时间"
                            ></el-table-column>
                            <el-table-column
                                    prop="comment"
                                    label="评价"
                                    width="400"
                            ></el-table-column>
                            <!--                            <el-table-column-->
                            <!--                                    prop="type"-->
                            <!--                                    label="类型"-->
                            <!--                            ></el-table-column>-->
                            <el-table-column
                                    fixed="right"
                                    label="操作"
                                    width="160">
                                <template slot-scope="scope">
                                    <el-button round size="small" @click="handleComment(scope.row)" >评价</el-button>
                                    <el-button round size="small" @click="handleDelete(scope.row)">删除</el-button>
                                </template>
                            </el-table-column>
                        </el-table-column>
                    </el-table>
                    <el-dialog title="输入评价" :visible.sync="dialogVisible">
                        <el-input
                                v-model="comment"
                                clearable
                                placeholder="请填写描述"
                                @input="change($event)"
                        />
<!--                        <el-input-->
<!--                                style="margin: 30px"-->
<!--                                v-model="rate"-->
<!--                                maxlength="8"-->
<!--                                placeholder="请选择整体分数"-->
<!--                                @input="change($event)"-->
<!--                        />-->
                        <div class="block">
                            <span class="demonstration">选择整体分数</span>
                            <el-slider v-model="rate"></el-slider>
                        </div>
                        <span slot="footer" class="dialog-footer">
                        <el-button @click="dialogVisible = false">取 消</el-button>
                        <el-button type="primary" @click="pushComment()">确 定</el-button>
                        </span>
                    </el-dialog>
                </el-main>
            </el-container>
<!--            <list style="margin-top: 30px"></list>-->
        </div>
        <div style="margin-top: 30px">
            <el-input v-model="search" style="width: 300px"
                      placeholder="请输入专家姓名" prefix-icon="el-icon-search"/>
            <el-table :data="candidate.filter(data=>!search || data.name.includes(search))"
                      stripe
                      style="width: 100%">
                <el-table-column>
                    <!--<template slot="header"  scope="scope" >-->
                    <!--<el-input v-model="search" style="width: 300px"-->
                    <!--placeholder="请输入书名" prefix-icon="el-icon-search"/>-->
                    <!--</template>-->
                    <el-table-column
                            prop="id"
                            label="ID"
                    >
                    </el-table-column>
                    <el-table-column
                            prop="name"
                            label="姓名"
                    ></el-table-column>
                    <el-table-column
                            prop="gender"
                            label="性别"
                    ></el-table-column>
                    <el-table-column
                            prop="birth"
                            label="生日"
                            sortable
                    ></el-table-column>
                    <el-table-column
                            prop="type"
                            label="类型"
                            sortable
                    ></el-table-column>
                    <el-table-column
                            prop="area"
                            label="领域"
                            sortable
                    ></el-table-column>
                    <el-table-column
                            prop="company"
                            label="单位"
                            sortable
                    ></el-table-column>
                    <el-table-column
                            prop="secret"
                            label="是否机密"
                    ></el-table-column>
                    <el-table-column
                            fixed="right"
                            label="操作"
                            width="150">
                        <template slot-scope="scope">
                            <el-button size="small" round @click="handleClick(scope.row)" >查看</el-button>
                            <el-button size="small" round @click="handlePick(scope.row)">选择</el-button>
                        </template>
                    </el-table-column>
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
    import navi from "../../components/navi";
    // import list from "../../components/pickList";
    export default {
        name: "program",
        components: {navi},
        data(){
            return{
                tableData:[],
                candidate:[],
                search:'',
                // programId:'',
                type:'技术',
                area:'核工程',
                keyword:'',
                number:'',
                time:'',
                company:'',
                state: '',
                dialogVisible:false,
                commentID:'',
                comment:'',
                rate:''
            }
        },
        mounted: function () {
            this.load()
        },
        methods: {
            change () {
                this.$forceUpdate()
            },
            load(){
                let that=this
                var url = 'http://localhost:8080/program/find/' + this.$route.query.id
                this.$http.get(url, {
                    headers: {
                        'Access-Control-Allow-Credentials': true,
                        'Access-Control-Allow-Origin': true
                    }
                })
                    .then(res => {
                        console.log(res.data)
                        // console.log(this)
                        // this.programID=res.data.id
                        that.number = res.data.number
                        that.time = res.data.time
                        that.type = res.data.type
                        that.area = res.data.area
                        that.keyword = res.data.keyword
                        that.company = res.data.company
                        that.state = res.data.state
                        console.log(res.data.type)
                        // console.log(that.isbn)
                    }).catch(error => {
                    JSON.stringify(error)
                    console.log(error)
                })
                this.$http.get('http://localhost:8080/record/getP/'+ this.$route.query.id).then((res) => {
                    this.tableData = res.data
                    console.log(res.data)
                }).catch(function (err) {
                    alert(err)
                })
                this.$http.get('http://localhost:8080/expert/get_all').then((res) => {
                    this.candidate = res.data
                    // console.log(res.data)
                }).catch(function (err) {
                    alert(err)
                })
            },
            edit(){
                this.$router.push({path: '/editExpert'})

            },
            handleComment(row){
                this.dialogVisible=true
                this.commentID=row.id
            },
            pushComment(){
                this.dialogVisible = false
                // var url = 'http://localhost:8080/record/get/'+this.commentID
            },
            handleDelete(row){
                var url = 'http://localhost:8080/record/delete/'+row.id
                this.$http({
                    method: 'delete',
                    url: url,
                    headers: {
                        'Access-Control-Allow-Credentials': true,
                        'Access-Control-Allow-Origin': true
                    }
                })
                    .then(response => {
                        console.log(response.data)
                    })
                    .catch(error => {
                        JSON.stringify(error)
                        console.log(error)
                    })
                this.$router.go(0)
                this.$notify({
                    title: '提醒',
                    message: '已移除此专家',
                    type: 'warning'
                });
            },
            handleClick(row){
                this.$router.push({path: '/expertDetail', query: {id: row.id}})
            },
            handlePick(row) {
                let data = {
                    expertID: row.id,
                    programID: this.$route.query.id,
                    time:this.time,
                }
                console.log(data)
                var url = 'http://localhost:8080/record/insert/'
                this.$http({
                    method: 'post',
                    url: url,
                    headers: {
                        'Access-Control-Allow-Credentials': true,
                        'Access-Control-Allow-Origin': true,
                        'Content-Type': 'application/json'
                    },
                    data: JSON.stringify(data)
                })
                    .then(response => {
                        console.log(response.data)
                        console.log('get response')
                        //redirect
                        // this.$router.push({path: '/programDetail', query: {id: this.$route.query.id}})
                        this.$router.go(0)
                    })
                    .catch(error => {
                        JSON.stringify(error)
                        console.log(error)
                    })
            }
        }
    }
</script>

<style scoped>
    .el-aside {
        background-color: #D3DCE6;
        /*margin-top: 30px;*/
        /*margin-left: 10px;*/
        color: #333;
        text-align: center;
        line-height: 30px;
    }

    .el-main {
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        line-height: 20px;
    }

    body > .el-container {
        margin-bottom: 40px;
    }

    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }

    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }
</style>