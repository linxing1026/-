<template>
    <div class="bind-wrap">
        <img src="./../../assests/手机绑定_banner.png" alt="">
        <div class="bind-main">
            <div class="phone">
                <input type="number" :placeholder="formData.tel.placeholder" v-model="formData.tel.value">
                <span class="get-code" :class="{'code-enabled':flags.canSendCode}" v-if="!countDownShow">获取验证码</span>
                <span class="get-code" v-if="countDownShow">重新获取验证码</span>
                <span class="ger-code" v-if="countDownShow">{{curTime}}s</span>
            </div>
            <div class="chaCode">
                <input type="number" :placeholder="formData.code.placeholder" v-model="formData.code.value">
            </div>
            <div class="bind-btn" :class="{'btn-enable':flags.canSubmit}">绑定手机</div>
        </div>

    </div>
</template>
<script>
    export default{
        data() {
            return {
                //参数验证
                notFill:"",
                fillError:"",
                formData:{
                    tel:{
                        value:"",
                        placeholder:"手机号码",
                        validate:(v)=>{
                            return /^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/.test(v);
                        }
                    },
                    code:{
                        value:"",
                        placeholder:"验证码",
                        validate:(v)=>{
                            if(v.length ==6) {
                                return true
                            }else {
                                return false
                            }
                        }
                    }
                },
                inputPhone:"",
                inputCode:"",
                curTime:60,
                classes:{
                    codeClass:"code-enabled ",
                    btnClass:"btn-enable"
                },
                flags: {
                    phoneSuccess:false,
                    countDownShow: false,
                    canSendCode: false,
                    canSubmit: false
                }
            }
        },
        watch:{
            formData:{
                deep:true,
                handler:function(n,o) {
                    this.checkParams();
                }
            },
            "formData.tel":{
                deep:true,
                handler:function(n,o) {
                    this.checkTel();
                    console.log(n,o);
                }
            }
        },
        methods:{
            countDown() {
                var timer = setInterval(()=>{
                    this.curTime--;
                    if(this.curTime<0) {
                        clearInterval(timer);
                        this.flags.countDownShow = !this.flags.countDownShow;
                    }
                },1000)
            },
            checkTel() {
                this.flags.canSendCode = false;
                var res = this.check(this.formData.tel.value,this.formData.tel.validate);
                if(res == "noFill") {
                    this.flags.canSendCode = false;
                }else if(res == "fillError") {
                    this.flags.canSendCode = false;
                }else if(res =="ok") {
                    this.flags.canSendCode = true;
                }else {
                    return
                }
            },
            checkParams() {
                var arr  =[this.formData.tel,this.formData.code];
                this.notFill = "";
                this.fillError="";
                for(let i=0;i<arr.length;i++) {
                    this.flags.canSubmit = false;
                    var res = this.check(arr[i].value,arr[i].validate);
                    if(res == "notFill") {
                        this.flags.canSubmit = false;
                    }else if(res =="fillError"){
                        this.flags.canSubmit = false;
                    }else if(res =="ok") {
                        this.flags.canSubmit = true;
                    }else {
                        return
                    }
                }
            },
            check(val, validateFn) {
                if(!val) {
                    return "notFill";
                }
                if(validateFn) {
                    if(validateFn(val)){
                        return 'ok';
                    }else {
                        return 'fillError';
                    }
                }else {
                    return 'ok';
                }
            }
        },
        onLaunch() {
            console.log(this.$root.$mp.query) //获取路由传过来的值
        }
    }
</script>

<style lang="less">
    .bind-wrap {
        img {
            width:100%;
            height:300rpx;
        }
        .bind-main {
            padding: 20rpx 30rpx 0 30rpx;
            .phone {
                padding: 50rpx 0 30rpx 0;
                border-bottom: 1px solid rgba(224,224,224,0.50);
                position: relative;
                input{
                    font-size: 16px;
                    color: #1E1E1E;
                    letter-spacing: 0;
                    height: 32rpx;
                    line-height: 32rpx;
                    width: 516rpx;
                    min-height: 0;
                    padding: 3px 0;
                    .input-placeholder {
                        color: #B4B4B4;
                        line-height: 16px;
                    }
                }
                .get-code {
                    line-height: 28rpx;
                    font-size: 14px;
                    position: absolute;
                    right: 30rpx;
                    top: 50rpx;
                    color: #888;
                }

            }
            .chaCode {
                padding: 50rpx 0 30rpx 0;
                border-bottom: 1px solid rgba(224,224,224,0.50);
                input{
                    font-size: 16px;
                    color: #1E1E1E;
                    letter-spacing: 0;
                    height: 32rpx;
                    line-height: 32rpx;
                    width: 516rpx;
                    min-height: 0;
                    padding: 3px 0;
                    .input-placeholder {
                        color: #B4B4B4;
                        line-height: 16px;
                    }
                }
            }
        }
        .bind-btn {
            width: 670rpx;
            line-height: 100rpx;
            border-radius: 6px;
            font-size: 18px;
            color: #FFFFFF;
            text-align: center;
            margin: 80rpx auto;
            background: #dcdcdc;
        }

    }

    .code-enabled {
        color: #E7438C!important;
    }

    .btn-enable {
        background: #E7438C!important;
    }

</style>