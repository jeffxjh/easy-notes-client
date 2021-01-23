<template>
    <div class="login" :style="backgourndStyle" >
      <a-spin tip="Loading..." size="large" :spinning="loading">
        <div style="height:120px">
          <a-row>
            <a-col>
              <a-col :span="1"></a-col>
              <a-col :span="7" ><my-icon type="icon-ningmeng" class="my-icon" style=" font-size:108px"/></a-col>
              <a-col :span="13"></a-col>
              <a-col :span="3"><my-icon type="icon-mianbao1" class="my-icon" style="margin-top:10px; font-size:108px"/></a-col>
            </a-col>
          </a-row>
        </div>
        <a-row type="flex">
          <a-col flex="auto"></a-col>
          <a-col flex="370px">
            <a-card hoverable
              :headStyle="{ fontSize: '22px' }">
              <div class="logo">
                <my-icon type="icon-GULULU-suanningmeng" />
                EasyNotes
              </div>
              <a-form :form="form" @submit="handleSubmit" class="login-form">
                <a-form-item>
                  <a-input
                      v-decorator="[
                      'userId',
                      {
                          rules: [
                          {
                              required: true,
                              min: 5,
                              max: 20,
                              message: '请输入账户名长度6-20!',
                              type: string
                          },
                          ],
                      },
                      ]"
                      placeholder="账户"
                  >
                    <a-icon slot="prefix" type="user" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item has-feedback>
                  <a-input
                      v-decorator="[
                      'password',
                      {
                          rules: [
                          {
                              required: true,
                              min: 8,
                              max: 40,
                              message: '请输入密码8-40!',
                              type: string
                          },
                          {
                              validator: validateToNextPassword,
                          },
                          ],
                      },
                      ]"
                      placeholder="输入密码"
                      type="password"
                  >
                    <a-icon slot="prefix" type="lock" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item has-feedback>
                  <a-input
                      v-decorator="[
                      'confirm',
                      {
                          rules: [
                          {
                              required: true,
                              min: 8,
                              max: 40,
                              message: '请输入密码确认密码!',
                              type: string
                          },
                          {
                              validator: compareToFirstPassword,
                          },
                          ],
                      },
                      ]"
                      type="password"
                      @blur="handleConfirmBlur"
                      placeholder="确认密码"
                  >
                    <a-icon slot="prefix" type="lock" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item>
                  <a-input
                      v-decorator="[
                      'userName',
                      {
                          rules: [{ required: true,min:6,max:50, message: '请输入昵称6-50!', whitespace: true, type: string }],
                      },
                      ]"
                      placeholder="昵称"
                  >
                    <a-icon slot="prefix" type="smile" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item>
                  <a-input
                      v-decorator="[
                      'email',
                      {
                          rules: [{ required: true, message: '请输入邮件!', whitespace: true, type: email  }, {validator: this.validateMobileMail}],
                      },
                      ]"
                      placeholder="邮件"
                  >
                    <a-icon slot="prefix" type="mail" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item>
                  <a-input
                      v-decorator="[
                      'phone',
                      {
                          rules: [{ required: true, message: '请输入手机!', whitespace: true ,type:number },{validator: this.validateMobilePhone}],
                      },
                      ]"
                      placeholder="手机"
                  >
                    <a-icon slot="prefix" type="phone" style="color: rgba(0,0,0,.25)" />
                  </a-input>
                </a-form-item>
                <a-form-item>
                  <a-button type="primary" html-type="submit" class="login-form-button">
                      注册
                  </a-button>
                </a-form-item>
                <a-form-item>
                  <div>
                    已有账户？<a @click="$router.push('/')"> 去登录  </a>
                  </div>
                </a-form-item>
              </a-form>
            </a-card>
          </a-col>
          <a-col flex="auto"></a-col>
        </a-row>
        <a-row>
          <a-col>
            <a-col :span="4"><my-icon type="icon-dangao" class="my-icon" style="margin:0px 50px 0px 0px; font-size:64px"/></a-col>
            <a-col :span="18" style="text-align:center">
              <my-icon type="icon-wing" class="my-icon" style="font-size:64px"/>
            </a-col>
            <a-col :span="2" style="text-align:right"><my-icon type="icon-zhuangshi2" class="my-icon" style=" font-size:64px"/></a-col>
          </a-col>
        </a-row>
      </a-spin>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        confirmDirty: false,
        loading: false
      };
    },
    computed:{
        backgourndStyle: function() {
            // 计算body可用高度
            let cHeight = window.outerHeight - (window.outerHeight - window.innerHeight)
            // 计算背景图
            // let imgs = ["http://resource.zealon.cn/01.jpg","http://resource.zealon.cn/02.jpg","http://resource.zealon.cn/03.jpg"]
            // let imgName = imgs[Math.floor(Math.random() * 3)]
            let imgName=this.getFileName()
            let style = "background-image:url('" + imgName + "'); background-repeat: round; height:" + cHeight + "px;";
            return style
        }
    },
    beforeCreate() {
      this.form = this.$form.createForm(this, { name: 'register' });
    },
    methods: {
        getFileName(){
        return '/img/'+Math.floor(Math.random()*12 + 1)+'.jpg';
    },  
      // 提交注册
      handleSubmit(e) {
        let that = this;
        e.preventDefault();
        this.form.validateFieldsAndScroll((err, values) => {
          if (!err) {
            this.loading = true;
            // this.postRequest('/register', values).then(resp => {
            //   that.loading = false
            //   if (resp && resp.code==200) {
            //     that.db.save("USER", resp.data);
            //     that.$router.replace('/home');
            //   }else{
            //     that.$message.info(resp.msg);
            //   }
            // })
              this.postRequest('/enableUserByEmail', values).then(resp => {
                console.log(values,111)
              that.loading = false
              if (resp && resp.code==200) {
                //that.db.save("USER", resp.data);
                that.$message.info("注册成功，去邮箱激活账号吧！");
                that.$router.replace('/');
              }else{
                that.$message.info(resp.msg);
              }
            })
          }
        });
      },
      handleConfirmBlur(e) {
        const value = e.target.value;
        this.confirmDirty = this.confirmDirty || !!value;
      },
      compareToFirstPassword(rule, value, callback) {
        const form = this.form;
        if (value && value !== form.getFieldValue('password')) {
          callback('两次密码不一致!');
        } else {
          callback();
        }
      },
      validateMobileMail(rule, value, callback){
        if (value === '') {
            callback(new Error('请正确填写邮箱'));
        } else {
              if (value !== '') { 
                var reg=/^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
              if(!reg.test(value)){
                    callback(new Error('请输入有效的邮箱'));
            }
          }
          callback();
        }
      },
      validateMobilePhone (rule, value, callback){
        if (value === '') {
          callback(new Error('负责人手机号不可为空'));
        } else {
          if (value !== '') { 
            var reg=/^1[3456789]\d{9}$/;
            if(!reg.test(value)){
              callback(new Error('请输入有效的手机号码'));
            }
          }
          callback();
        }
      },
      validateToNextPassword(rule, value, callback) {
        const form = this.form;
        if (value && this.confirmDirty) {
          form.validateFields(['confirm'], { force: true });
        }
        callback();
      }
    },
  };
</script>

<style>
  .login{
    width:100%;  
  }

  .login-form-button {
    width: 100%;
  }
</style>