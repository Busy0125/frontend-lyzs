<template>
  <el-row class="top_style">
    <el-col :span="20">
      <div class="box_head">
        <h2 class="box_head_title">数据上传</h2>
      </div>
      <el-tabs type="border-card">
        <div style="margin-left: 100px;">
          <el-col :span="20" class="context">
            <el-steps :active="active" finish-status="success">
              <el-step title="数据内容信息" icon="el-icon-edit"></el-step>
              <el-step title="数据用户信息" icon="el-icon-edit"></el-step>
              <el-step title="数据来源信息" icon="el-icon-upload"></el-step>
            </el-steps>
          </el-col>
          <el-col :span="20" class="context" v-show="active == 0">
            <el-form :model="dataContents" ref="form1" :rules="rules1">
              <el-row>
                <el-form-item label="数据摘要" :label-width="formLabelWidth" prop="d_abstract">
                  <el-input v-model="dataContents.d_abstract" maxlength="255"></el-input>
                </el-form-item>
              </el-row>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="样本数" :label-width="formLabelWidth" prop="d_size_m">
                    <el-input v-model="dataContents.d_size_m" maxlength="10"
                              oninput="value=value.replace(/[^\d]/g,'')"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="维度" :label-width="formLabelWidth" prop="d_size_n">
                    <el-input v-model="dataContents.d_size_n" maxlength="10"
                              oninput="value=value.replace(/[^\d]/g,'')"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-form-item label="关键词" :label-width="formLabelWidth" prop="keywords">
                  <el-input v-model="dataContents.keywords" maxlength="255"></el-input>
                </el-form-item>
              </el-row>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="数据领域类型" :label-width="formLabelWidth" prop="domain_type">
                    <el-select v-model="dataContents.domain_type" placeholder="--数据领域类型--">
                      <el-option label="高温合金数据" value="sa"></el-option>
                      <el-option label="锂电池数据" value="li-lon"></el-option>
                      <el-option label="引力波数据" value="gw"></el-option>
                      <el-option label="其他数据" value="other"></el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="方向类型" :label-width="formLabelWidth" prop="area_type">
                    <el-select v-model="dataContents.area_type" placeholder="--方向类型--">
                      <el-option label="蠕变" value="cp1"></el-option>
                      <el-option label="扩散" value="d2"></el-option>
                      <el-option label="热力学性能" value="tdp3"></el-option>
                      <el-option label="力学（弹性）性能" value="mp4"></el-option>
                      <el-option label="其他数据" value="other"></el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="20">
                  <el-form-item label="文件上传" :label-width="formLabelWidth">
                    <el-upload
                      class="upload-demo"
                      ref="upload"
                      action=""
                      :file-list="uploadForm.fileList"
                      :on-change="handleSelectFile"
                      :http-request="overSubmit"
                      :show-file-list="false"
                      accept=".xls,.xlsx,.cif,.vasp"
                      :auto-upload="false">
                      <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
                      <!--                        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器-->
                      <!--                        </el-button>-->
                      <span style="margin-left: 10px;" v-if="fileStatus === true">文件上传成功</span>
                      <div slot="tip" class="el-upload__tip">
                        (仅支持.xls、.xlsx、.cif、.vasp；文件数据格式：第一列为索引，最后一列为决策属性，第一行为属性名称)
                      </div>
                    </el-upload>
                  </el-form-item>
                </el-col>
              </el-row>
            </el-form>
          </el-col>
          <el-col :span="20" class="context" v-show="active == 1">
            <el-form :model="dataUser" ref="form2" :rules="rules2">
              <el-row>
                <el-col :span="10">
                  <el-form-item label="提交者" :label-width="formLabelWidth" prop="d_submission_name">
                    <el-input v-model="dataUser.d_submission_name" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="所在单位" :label-width="formLabelWidth" prop="d_submission_unit">
                    <el-input v-model="dataUser.d_submission_unit" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-form-item label="校对者" :label-width="formLabelWidth" prop="d_proofreader">
                <el-input v-model="dataUser.d_proofreader" maxlength="255"></el-input>
              </el-form-item>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="邮箱" :label-width="formLabelWidth" prop="email">
                    <el-input v-model="dataUser.email"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="电话" :label-width="formLabelWidth" prop="telephoneNumber">
                    <el-input v-model="dataUser.telephoneNumber" maxlength="11"
                              oninput="value=value.replace(/[^\d]/g,'')"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-form-item label="通讯地址" :label-width="formLabelWidth" prop="contact_address">
                <el-input v-model="dataUser.contact_address"></el-input>
              </el-form-item>
            </el-form>
          </el-col>
          <el-col :span="20" class="context" v-show="active == 2">
            <el-form :model="dataSource" ref="form3" :rules="rules3">
              <el-row>
                <el-col :span="10">
                  <el-form-item label="数据分类" :label-width="formLabelWidth" prop="data_sort">
                    <el-select v-model="dataSource.data_sort" placeholder="--数据分类--">
                      <el-option label="实验数据" value="1"></el-option>
                      <el-option label="计算数据" value="2"></el-option>
                      <el-option label="预测数据" value="3"></el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="决策属性" :label-width="formLabelWidth" prop="decide_type">
                    <el-input v-model="dataSource.decide_type" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="牌号" :label-width="formLabelWidth" prop="material_trademark">
                    <el-input v-model="dataSource.material_trademark" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="材料名称" :label-width="formLabelWidth" prop="m_name">
                    <el-input v-model="dataSource.m_name" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="实验条件名称" :label-width="formLabelWidth" prop="expcon_name">
                    <el-input v-model="dataSource.expcon_name" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="实验参数设置" :label-width="formLabelWidth" prop="exp_parasetting">
                    <el-input v-model="dataSource.exp_parasetting" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="10">
                  <el-form-item label="实验设备名称与型号" :label-width="formLabelWidth" prop="exp_device_name">
                    <el-input v-model="dataContents.exp_device_name" maxlength="255"></el-input>
                  </el-form-item>
                </el-col>
                <el-col :span="10" :offset="4">
                  <el-form-item label="数据来源" :label-width="formLabelWidth" prop="data_source">
                    <el-select v-model="dataSource.data_source" placeholder="--数据分类--">
                      <el-option label="文献" value="1"></el-option>
                      <el-option label="专利" value="2"></el-option>
                      <el-option label="其他" value="3"></el-option>
                    </el-select>
                  </el-form-item>
                </el-col>
              </el-row>
            </el-form>
          </el-col>
          <el-col :span="8" class="context">
            <el-button @click="lastStep()">上一步</el-button>
            <el-button @click="nextStepForData()">{{ btnName }}</el-button>
          </el-col>
        </div>
      </el-tabs>
      <div class="box_head">
        <h2 class="box_head_title">数据展示</h2>
      </div>
      <el-tabs type="border-card">
        <el-table :data="tableData" stripe border style="width: 100%">
          <el-table-column v-for="item in tableColumn" :prop="item" :label="item"></el-table-column>
        </el-table>
      </el-tabs>
    </el-col>
  </el-row>
</template>

<script>
import ElRow from "element-ui/packages/row/src/row";
import ElCol from "element-ui/packages/col/src/col";

import * as XLSX from 'xlsx'

export default {
  name: "AddData",
  data() {
    // 自定义邮箱规则
    let checkEmail = (rule, value, callback) => {
      const regEmail = /^\w+@\w+(\.\w+)+$/
      if (regEmail.test(value)) {
        // 合法邮箱
        return callback()
      }
      callback(new Error('请输入合法邮箱'))
    }
    // 自定义手机号规则
    let checkMobile = (rule, value, callback) => {
      const regMobile = /^1[34578]\d{9}$/
      if (regMobile.test(value)) {
        return callback()
      }
      // 返回一个错误提示
      callback(new Error('请输入合法的手机号码'))
    }

    return {
      formLabelWidth: '120px',
      active: 0,
      btnName: '下一步',

      dataContents: {
        d_abstract: '',
        d_size_m: '',
        d_size_n: '',
        keywords: '',
        domain_type: '',
        area_type: '',
      },
      dataUser: {
        d_submission_name: '',
        d_submission_unit: '',
        d_proofreader: '',
        email: '',
        telephoneNumber: '',
        contact_address: ''
      },
      dataSource: {
        data_sort: '',
        decide_type: '',
        material_trademark: '',
        m_name: '',
        expcon_name: '',
        exp_parasetting: '',
        exp_device_name: '',
        data_source: ''
      },
      file_attachment: '',
      rules1: {
        d_size_m: [
          {required: true, message: '请输入样本数', trigger: 'blur'},
        ],
        d_size_n: [
          {required: true, message: '请选择维度', trigger: 'blur'},
        ],
        keywords: [
          {required: true, message: '请输入关键字', trigger: 'blur'},
        ],
        domain_type: [
          {required: true, message: '请选择数据领域类型', trigger: 'change'},
        ],
        area_type: [
          {required: true, message: '请选择方向类型', trigger: 'change'},
        ]
      },
      rules2: {
        d_submission_name: [
          {required: true, message: '请输入提交者', trigger: 'blur'},
        ],
        d_proofreader: [
          {required: true, message: '请输入校对者', trigger: 'blur'},
        ],
        email: [
          {required: true, message: '请输入邮箱', trigger: 'blur'},
          {validator: checkEmail, trigger: 'blur'}
        ],
        telephoneNumber: [
          {required: true, message: '请输入电话', trigger: 'blur'},
          {validator: checkMobile, trigger: 'blur'}
        ],
        contact_address: [
          {required: true, message: '请输入通讯地址', trigger: 'blur'},
        ]
      },
      rules3: {
        data_sort: [
          {required: true, message: '请选择数据分类', trigger: 'change'},
        ],
        data_source: [
          {required: true, message: '请输入数据来源', trigger: 'change'},
        ]
      },
      uploadForm: {
        inputFile: '',
        fileList: [],
      },
      fd: null,
      fileStatus: false,

      tableData: [],
      tableColumn: []
    }
  },
  mounted: function () {

  },

  methods: {
    lastStep() {
      if (this.active < 1) {
        this.active = 0;
      } else {
        this.btnName = '下一步';
        this.active--;
      }
    },

    nextStepForData() {
      if (this.active === 0) {
        this.$refs['form1'].validate((valid) => {
          if (valid) {
            if (this.fileStatus === false) {
              this.$message.error("请上传合适的文件");
              return;
            }
            this.active++;
            if (this.active === 2) {
              this.btnName = '提交';
            } else if (this.active > 2) {
              this.active = 2;
              this.addNewData();
            }
          }
        })
      } else if (this.active === 1) {
        this.$refs['form2'].validate((valid) => {
          if (valid) {
            this.active++;
            if (this.active === 2) {
              this.btnName = '提交';
            } else if (this.active > 2) {
              this.active = 2;
              this.addNewData();
            }
          }
        })
      } else if (this.active === 2) {
        this.$refs['form3'].validate((valid) => {
          if (valid) {
            this.active++;
            if (this.active === 2) {
              this.btnName = '提交';
            } else if (this.active > 2) {
              this.active = 2;
              this.submitUpload();
            }
          }
        })
      }
    },

    addNewData() {
      let params = {
        d_abstract: this.dataContents.d_abstract,
        d_size_m: this.dataContents.d_size_m,
        d_size_n: this.dataContents.d_size_n,
        keywords: this.dataContents.keywords,
        domain_type: this.dataContents.domain_type,
        area_type: this.dataContents.area_type,

        d_submission_name: this.dataUser.d_submission_name,
        d_submission_unit: this.dataUser.d_submission_unit,
        d_proofreader: this.dataUser.d_proofreader,
        email: this.dataUser.email,
        telephoneNumber: this.dataUser.telephoneNumber,
        contact_address: this.dataUser.contact_address,

        file_attachment: this.file_attachment,
      };
      let params2 = {
        data_sort: this.dataSource.data_sort,
        decide_type: this.dataSource.decide_type,
        material_trademark: this.dataSource.material_trademark,
        m_name: this.dataSource.m_name,
        expcon_name: this.dataSource.expcon_name,
        exp_parasetting: this.dataSource.exp_parasetting,
        exp_device_name: this.dataSource.exp_device_name,
        data_source: this.dataSource.data_source
      }
      this.$axios.post('/demo/addNewData', params).then(res => {
        if (res.data.code === 1) {
          this.$axios.post("/demo/addNewExp", params2).then(res2 => {
            if (res2.data.code === 1) {
              this.$notify({
                title: '成功',
                message: res2.data.msg,
                type: 'success'
              });
              this.exportData();
            } else {
              this.$notify({
                title: '警告',
                message: res2.data.msg,
                type: 'warning'
              });
            }
          })
        } else {
          this.$notify({
            title: '警告',
            message: res.data.msg,
            type: 'warning'
          });
        }
      })
    },
    handleSelectFile(file, fileList) {
      if (fileList.length > 1) {
        fileList.shift();
      }
      this.uploadForm.inputFile = file.name;

      this.fileStatus = true;
      const fileSuffix = file.name.substring(file.name.lastIndexOf(".") + 1);
      const whiteList = ["xls", "xlsx", "cif", "vasp"];
      if (whiteList.indexOf(fileSuffix) === -1) {
        this.$message.error('仅支持.xls、.xlsx、.cif、.vasp');
        this.fileStatus = false;
      }

      // const isLt2M = file.size / 1024 / 1024 < 2;
      // if (!isLt2M) {
      //   this.$message.error('上传文件大小不能超过 2MB');
      //   this.fileStatus= false;
      // }
    },
    overSubmit(param) {
      this.fd = new FormData();
      this.fd.append('file', param.file);
    },
    submitUpload() {
      if (this.fileStatus) {
        this.$refs.upload.submit();
        this.$axios.post('/demo/selectFileUpload', this.fd).then(res => {
          console.log(res);
          this.file_attachment = res.data.data;
          if (res.data.code === 1) {
            this.fileStatus = false;
            this.uploadForm.inputFile = '';
            this.addNewData();
          } else {
            this.$notify({
              title: '警告',
              message: res.data.msg,
              type: 'warning'
            });
          }
        })
      } else {
        this.$message.error('请先上传合适的文件');
      }
    },

    exportData() {
      console.log("展示")
      let f = this.fd.get("file");
      let reader = new FileReader();
      const that = this;
      reader.onload = function (e) {
        let data = e.target.result;
        let wb = XLSX.read(data, {
          type: 'buffer',
        })
        let outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]);

        console.log(outdata);
        console.log(Object.keys(outdata[0]).length);
        that.tableData = outdata.slice(0, Object.keys(outdata[0]).length - 1);

        let importDataThead = Array.from(Object.keys(outdata[0])).map(
          (item) => {
            return item;
          }
        )

        that.tableColumn = importDataThead.slice(0, Object.keys(outdata[0]).length)
        console.log(that.tableData);
        console.log(that.tableColumn);
      }
      reader.readAsArrayBuffer(f);
    }
  },
}
</script>

<style scoped>
.top_style {
  margin: 5px;
}

.dataHead {
  position: absolute;
  top: 54%;
  left: 55%;
  transform: translate(-50%, -50%);
  width: 70%;
  height: 80%;
  border: 2px solid #adcadd;
  border-top: none;
  border-radius: 5px;
  background-color: #fff;
}

.box_head {
  position: relative;
  width: 100%;
  height: 45px;
  border-bottom: 1px solid #adcadd;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
  background-color: #dfeef8;
}

.box_head_title {
  height: 45px;
  line-height: 45px;
  margin-left: 10px;
  font-size: 14px;
  /* color: #fff; */
}

.context {
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
