<template>
    <div class="header">
        <div class="logo">视频监控管理系统</div>
        <div class="login-name"><span>登录人:</span>&nbsp;<span>{{currUserName}}</span></div>
        <div class="login-name"><span>当前部门:</span>&nbsp;<span>{{currDeptName}}</span></div>
        <div class="user-info">
            <el-dropdown trigger="click" @command="handleCommand">
                <span class="el-dropdown-link">
                   <img src="../../../static/img/loginOut.png" alt="" width="25">
                </span>
                <el-dropdown-menu slot="dropdown" placement="top-end">
                    <el-dropdown-item command="logout">退出</el-dropdown-item>
                    <el-dropdown-item command="userInfo">个人信息</el-dropdown-item>
                    <el-dropdown-item command="updatePassword">修改密码</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
        </div>
        <div class="headerTimer">
        </div>
    </div>
</template>
<script>
    export default {
        computed:{
            currUserName(){
                return this.getUserInfo().userName;
            },
            currDeptName(){
                return this.getUserInfo().deptName;
            }
        },
        methods:{
            getUserInfo(){
                // return JSON.parse(this.$cookie.get('userInfo'));
                return JSON.parse(sessionStorage.getItem('userInfo'));
            },
            handleCommand(command) {
                if(command == 'logout'){
                    this.$http.get(this.$global.remote().logout,null,response => {
                        this.clearCookie();
                        this.clearStore();
                        //不需要router
                        // sessionStorage.removeItem('routers');
                        // this.$router.push('/login');
                        //flash
                        window.location.reload();
                    });
                } else if (command == 'userInfo') {

                } else if (command == 'updatePassword'){

                }

            },
            clearCookie : function(){
                this.$cookie.set('userName','',-1);
                // this.$cookie.set('userInfo','',-1);
                // this.$cookie.set('userMenu','',-1);
                sessionStorage.removeItem('userInfo');
                sessionStorage.removeItem('userMenu');
            },
            clearStore : function(){
                this.$store.commit("clearStore");
            },
            autoTime(){
                var timeEle=document.querySelector('.headerTimer');
                setInterval(function () {
                    let time=new Date();
                    var year=time.getFullYear();
                    var month=time.getMonth()>=9?(time.getMonth()+1):("0"+(time.getMonth()+1));
                    var day=time.getDate()>9?time.getDate():"0"+(time.getDate());
                    var hour=time.getHours()>9?time.getHours():"0"+(time.getHours());
                    var minute=time.getMinutes()>9?time.getMinutes():"0"+(time.getMinutes());
                    var second=time.getSeconds()>9?time.getSeconds():"0"+(time.getSeconds());
                    timeEle.innerHTML=year+"-"+month+"-"+day+" "+hour+"-"+minute+"-"+second;
                },1000)
            }
        },
        mounted:function () {
            this.autoTime();
        }
    }
</script>
<style scoped>
    .header {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        height: 70px;
        font-size: 22px;
        line-height: 70px;
        color: #fff;
    }
    .header .logo{
        float: left;
        width:250px;
        text-align: center;
    }
    .user-info {
        float: right;
        padding-right: 75px;
        font-size: 16px;
        color: #fff;
    }
    .user-info .el-dropdown-link{
        position: relative;
        display: inline-block;
        color: #fff;
        cursor: pointer;
        vertical-align: middle;
    }
    .user-info .user-logo{
        position: absolute;
        left:0;
        top:15px;
        width:40px;
        height:40px;
        border-radius: 50%;
    }
    .el-dropdown-menu__item{
        text-align: center;
    }
    .el-dropdown-link>img{
        vertical-align: middle;
        margin-top:-5px;
    }
    .login-name{
        float: left;
        font-size:18px;
    }
    .login-name>span:nth-child(1){
        font-weight: bold;
    }
    .login-name>span:nth-child(2){
        color:#F15B23;
    }
    .header>.login-name:nth-child(3){
        margin-left: 20px;
    }
    .headerTimer{
        float:right;
        padding-right: 10px;
        font-weight: 300;
        font-size:20px;
    }
</style>
