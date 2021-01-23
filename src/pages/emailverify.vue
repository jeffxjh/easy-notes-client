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
  
    </a-spin>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false
      };
    },
    computed:{
        backgourndStyle: function() {
            // 计算body可用高度
            let cHeight = window.outerHeight - (window.outerHeight - window.innerHeight)
            let imgName=this.getFileName()
            let style = "background-image:url('" + imgName + "'); background-repeat: round; height:" + cHeight + "px;";
            return style
        }
    },
    created(){
      console.log('created')
      let id = this.$route.params.id
      console.log(id)
      this.checkEmail(id)
    },
    beforeCreate() {
      console.log('beforeCreate')
      this.form = this.$form.createForm(this, { name: 'normal_login' });
    },
    mounted () {
      let user = this.db.get("USER")
      console.log(user)
      if(user!=null && user.token!=null){
        this.$router.replace('/home');
      }
    },
    methods: {
      getFileName(){
        return '/img/'+Math.floor(Math.random()*12 + 1)+'.jpg';
    }, 
      checkEmail(id){
       
          let values = 
            {
              emailToken: id,
            }
          let that = this;
          this.getRequest('/activateMail', values).then(resp => {
            that.loading = false
          if (resp && resp.code==200) {
              that.db.save("USER", resp.data);
              that.$router.replace('/home');
          }else if (resp && resp.code===3005) {
            //链接失效
              that.$message.info(resp.msg);
              that.$router.replace('/register');
          }else if (resp && resp.code===3006) {
            //账号已注册
              that.$message.info(resp.msg);
              that.$router.replace('/');
          }else{
              that.$message.info(resp.msg);
              that.$router.replace('/register');
          }
          })
      }

    },
  };
</script>
<style>
  .logo {
    height: 32px;
    color: #82c090;
    font: 300;
    font-size: 36px;
    text-align: center;
  }

  .login{
    width:100%;  
  }

  .login-form {
    margin-top: 45px
  }

  .login-form-button {
    width: 100%;
  
  }

  .my-icon {
    font-size: 48px;
  }

  .my-icon .s72{
    font-size: 72px;
  }

  .my-icon .s108{
    font-size: 108px;
  }

  .my-icon .s128{
    font-size: 128px;
  }

  .my-icon .s156{
    font-size: 156px;
  }

  .my-icon .s256{
    font-size: 256px;
  }
</style>