<template>
  <div>
    <el-row class="box">
      <div align="center" style="margin-top: 20px">
        <el-col :span="10" :offset="6">
          <h1 class="title">用户信息</h1>
        </el-col>
        <el-col :span="10" :offset="6">
          <el-form :label-position="labelPosition" label-width="100px" :model="formUserInfo">
            <el-form-item label="用户名">
              <label>{{formUserInfo.userName}}</label>
            </el-form-item>
            <el-form-item label="邮箱">
              <label>{{formUserInfo.email}}</label>
            </el-form-item>
            <el-form-item label="电话">
              <label>{{formUserInfo.telephone}}</label>
            </el-form-item>
            <el-form-item label="统一认证号">
              <label>{{formUserInfo.loginNo}}</label>
            </el-form-item>
            <el-form-item label="角色">
              <label>{{formUserInfo.role}}</label>
            </el-form-item>
            <el-form-item label="归属平台">
              <label>{{formUserInfo.platform}}</label>
            </el-form-item>
            <el-form-item label="用户信息">
              <label>{{formUserInfo.userInfo}}</label>
            </el-form-item>
            <el-form-item label="职业">
              <label>{{formUserInfo.occupation}}</label>
            </el-form-item>
            <el-form-item label="研究领域">
              <label>{{formUserInfo.researchDomain}}</label>
            </el-form-item>
            <el-form-item label="研究方向">
              <label>{{formUserInfo.researchDirection}}</label>
            </el-form-item>
            <el-form-item label="工作单位">
              <label>{{formUserInfo.workOrganization}}</label>
            </el-form-item>
            <el-form-item label="居住地址">
              <label>{{formUserInfo.address1}}</label>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="cUserInfo">修改用户信息</el-button>
              <el-button @click="dialogResetPassword=true">重置密码</el-button>
            </el-form-item>
          </el-form>
        </el-col>
      </div>
    </el-row>

    <el-dialog title="修改用户信息" :visible.sync="dialogChangeUserInfo" :modal=false @close="closeChangeUserInfoDialog">
      <el-form :model="formChangeUserInfo" :rules="userInfoRules" ref="formChangeUserInfo">
        <el-form-item label="新的用户描述信息" :label-width="formLabelWidth" prop="userInfo">
          <el-input v-model="formChangeUserInfo.userInfo" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="职业" :label-width="formLabelWidth">
          <el-input v-model="formChangeUserInfo.occupation" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="研究领域" :label-width="formLabelWidth">
          <el-input v-model="formChangeUserInfo.researchDomain" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="研究方向" :label-width="formLabelWidth">
          <el-input v-model="formChangeUserInfo.researchDirection" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="工作单位" :label-width="formLabelWidth">
          <el-input v-model="formChangeUserInfo.workOrganization" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="居住地址" :label-width="formLabelWidth">
          <el-input v-model="formChangeUserInfo.address1" style="width: 80%"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancelChangeUserInfo">取 消</el-button>
        <el-button type="primary" @click="changeUserInfo">确 定</el-button>
      </div>
    </el-dialog>

    <el-dialog title="修改密码" :visible.sync="dialogResetPassword" :modal=false>
      <el-form :model="formResetPassword">
        <el-form-item label="新密码" :label-width="formLabelWidth">
          <el-input v-model="formResetPassword.newPassWord" style="width: 80%"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogResetPassword = false">取 消</el-button>
        <el-button type="primary" @click="resetPassword">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>

  import ElRow from "element-ui/packages/row/src/row";

  export default {
    components: {ElRow},
    name: "UserInfo",
    data() {
      return {
        labelPosition: 'left',
        formUserInfo: {
          userName: '',
          email: '',
          telephone: '',
          loginNo: '',
          userInfo: '',
          role: '',
          platform: '',
          occupation: '',
          researchDomain: '',
          researchDirection: '',
          workOrganization: '',
          address1: ''
        },
        formLabelWidth: '200px',
        dialogChangeUserInfo: false,
        dialogResetPassword: false,
        formChangeUserInfo: {
          userInfo: '',
          occupation: '',
          researchDomain: '',
          researchDirection: '',
          workOrganization: '',
          address1: '',
        },
        formResetPassword: {
          newPassWord: '',
        },
        userInfoRules: {
          userInfo: [
            {required: true, message: '请输入新的用户描述信息', trigger: 'blur'}
          ],
        }
      }

    },
    mounted: function () {

      this.getUserInfo()
//    this.formChangeUserInfo.newUserInfo=this.formUserInfo.userInfo


    },
    methods: {
      getUserInfo() {
        let params = {
//        loginNo: localStorage.getItem("loginNo"),
        };
        this.$axios.post('/user/getUserInfo', params).then(res => {
          if (res.data.code === 1) {
            let JsonParams = JSON.parse(res.data.data);
            this.formUserInfo.userName = JsonParams.userName;
            this.formUserInfo.userInfo = JsonParams.email;
            this.formUserInfo.userInfo = JsonParams.telephone;
            this.formUserInfo.loginNo = JsonParams.loginNo;
            this.formUserInfo.role = JsonParams.role;
            this.formUserInfo.platform = JsonParams.platform;
            this.formUserInfo.userInfo = JsonParams.userInfo;
            this.formUserInfo.userInfo = JsonParams.occupation;
            this.formUserInfo.userInfo = JsonParams.researchDomain;
            this.formUserInfo.userInfo = JsonParams.researchDirection;
            this.formUserInfo.userInfo = JsonParams.workOrganization;
            this.formUserInfo.userInfo = JsonParams.address1;
          } else {
            this.$notify({
              title: '警告',
              message: res.data.msg,
              type: 'warning'
            });
          }

        })
      },
      cUserInfo() {
        this.dialogChangeUserInfo = true;
        this.formChangeUserInfo.userInfo = this.formUserInfo.userInfo
        this.formChangeUserInfo.occupation = this.formUserInfo.occupation
        this.formChangeUserInfo.researchDomain = this.formUserInfo.researchDomain
        this.formChangeUserInfo.researchDirection = this.formUserInfo.researchDirection
        this.formChangeUserInfo.workOrganization = this.formUserInfo.workOrganization
        this.formChangeUserInfo.address1 = this.formUserInfo.address1

      },
      closeChangeUserInfoDialog() {
        this.$refs['formChangeUserInfo'].resetFields();
      },
      changeUserInfo() {
        this.$refs['formChangeUserInfo'].validate((valid) => {
          if (valid) {
            let params = {
              userInfo: encodeURIComponent(this.formChangeUserInfo.userInfo),
              occupation: encodeURIComponent(this.formChangeUserInfo.occupation),
              researchDomain: encodeURIComponent(this.formChangeUserInfo.researchDomain),
              researchDirection: encodeURIComponent(this.formChangeUserInfo.researchDirection),
              workOrganization: encodeURIComponent(this.formChangeUserInfo.workOrganization),
              address1: encodeURIComponent(this.formChangeUserInfo.address1),
            };
            this.$axios.post('/user/changeUserInfo', params).then(res => {
              if (res.data.code === 1) {
                this.$notify({
                  title: '成功',
                  message: res.data.msg,
                  type: 'success'
                });
                this.formUserInfo.userInfo = this.formChangeUserInfo.userInfo;
                this.dialogChangeUserInfo = false
              } else {
                this.$notify({
                  title: '警告',
                  message: res.data.msg,
                  type: 'warning'
                });
              }
            })
          } else {
            this.$notify({
              title: '警告',
              message: '输入不符合要求',
              type: 'warning'
            });
            return false;
          }
        });
      },
      cancelChangeUserInfo() {
        this.dialogChangeUserInfo = false;
//      this.formChangeUserInfo.newUserInfo=this.formUserInfo.userInfo
      },
      resetPassword() {
        let reg = /^[0-9a-zA-Z]{1,20}$/;
        if (this.formResetPassword.newPassWord === '') {
          this.$notify({
            title: '提示',
            message: '密码不能为空!',
            type: 'warning'
          });
        } else if (!(reg.test(this.formResetPassword.newPassWord))) {
          this.$notify({
            title: '提示',
            message: '密码长度为1到20个字母数字!',
            type: 'warning'
          });
        } else {
          let params = {
            newPassword: this.$md5(this.formResetPassword.newPassWord),
          };
          this.$axios.post('/user/resetPassword', params).then(res => {
            if (res.data.code === 1) {
              this.$notify({
                title: '成功',
                message: res.data.msg + ',请重新登录',
                type: 'success'
              });
              this.dialogResetPassword = false;
              this.$router.push({path: "/login"});
              //清理session
              this.signOut();
            } else {
              this.$notify({
                title: '警告',
                message: res.data.msg,
                type: 'warning'
              });
            }
          })
        }
      },
      signOut() {
        let params = {};
        this.$axios.post('/user/signOut', params).then(res => {
          if (res.data.code === 1) {
            //nothing
          } else {
            this.$notify({
              title: '警告',
              message: res.data.msg,
              type: 'warning'
            });
          }
        })
      },
    }
  }
</script>

<style scoped>
  .title {
    font-size: 20px;
    border-bottom: 1px solid #eee;
    padding-bottom: 5px;
  }
</style>
