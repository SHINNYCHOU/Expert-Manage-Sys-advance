<template>
    <div>
        <navi></navi>
        <div style="margin-top: 10px">
            <el-form  label-width="100px">
                <el-row :gutter="20">
                    <el-col :span="6">
                        <el-form-item label="姓名">
                            <el-input v-model="name" ></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="4">
                        <el-form-item label="性别">
                            <el-select v-model="gender" clearable placeholder="请选择">
                                <el-option label="男" value="男"></el-option>
                                <el-option label="女" value="女"></el-option>
                                <el-option label="其他" value=" "></el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="6">
                        <el-form-item label="是否机密">
                            <el-select v-model="secret" placeholder="请选择">
                                <el-option
                                        v-for="item in secretoptions"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
<!--                    <el-col :span="6" style="align-items: end">-->
<!--                        <el-form-item label="电话">-->
<!--                            <el-input v-model="phone" ></el-input>-->
<!--                        </el-form-item>-->
<!--                    </el-col>-->
                </el-row>
                <el-form-item label="单位">
                    <el-input v-model="company" ></el-input>
                </el-form-item>
                <el-row :gutter="20">
                    <el-col :span="6">
                        <el-form-item label="出生日期">
                            <el-date-picker
                                    v-model="birth"
                                    type="date"
                                    placeholder="选择日期">
                            </el-date-picker>
                        </el-form-item>
                    </el-col>
                    <el-col :span="6">
                        <el-form-item label="类型">
<!--                            <el-input v-model="Form.type" ></el-input>-->
                            <el-select v-model="type" placeholder="请选择">
                                <el-option label="学术" value="学术"></el-option>
                                <el-option label="财务" value="财务"></el-option>
                                <el-option label="法律" value="法律"></el-option>
                                <el-option label="其他" value="其他"></el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="9">
                        <el-form-item label="领域">
                            <el-input v-model="area" ></el-input>
                        </el-form-item>
                    </el-col>
                </el-row>
                <el-form-item>
                    <el-button type="primary" @click="submitForm()">确认</el-button>
                    <el-button @click="resetForm()">取消</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import navi from "../../components/navi";
    export default {
        name: "edit",
        components: {navi},
        data(){
            return{
                    name:'',
                    phone:'',
                    gender:'',
                    secret:'',
                    company:'',
                    type:'',
                    area:'',
                    birth:'',
                secretoptions: [{
                    value: 'true',
                    label: '机密人员'
                }, {
                    value: 'false',
                    label: '非机密人员'
                }],
                value: ''
            }
        },
        mounted: function () {
            this.load()
        },
        methods:{
            load() {
                let that = this
                var url = 'http://localhost:8080/expert/get/' + this.$route.query.id
                this.$http.get(url, {
                    headers: {
                        'Access-Control-Allow-Credentials': true,
                        'Access-Control-Allow-Origin': true
                    }
                })
                    .then(res => {
                        // console.log(res.data)
                        that.name = res.data.name
                        that.gender = res.data.gender
                        that.birth =res.data.birth
                        that.type = res.data.type
                        that.area = res.data.area
                        that.company = res.data.company
                        that.secret = res.data.secret
                        console.log(res.data.store)
                        // console.log(that.isbn)
                    }).catch(error => {
                    JSON.stringify(error)
                    console.log(error)
                })
            },
            resetForm(){
                this.$router.push({path: '/expertDetail', query: {id: this.$route.query.id}})
            },
            submitForm(){
                // let that = this
               let data = {
                   id:this.$route.query.id,
                    name: this.name,
                    phone: this.phone,
                   gender: this.gender,
                   secret: this.secret,
                   company:this.company,
                   type: this.type,
                   area:this.area,
                   birth: this.birth
                }
                var url = 'http://localhost:8080/expert/insert/'
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
                        this.$router.push({path: '/expert'})
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

</style>