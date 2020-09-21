<template>
  <el-card class="form-user" shadow="never">
    <h4>基础信息</h4>
    <div>
      <el-form :model="ruleForm" :rules="rules" ref="productInfoForm" label-width="120px"
               size="small">
        <el-form-item label="账号：" prop="name">
          <el-input v-model="ruleForm.name" placeholder="请输入账号"></el-input>
        </el-form-item>
        <el-form-item label="密码：" prop="password">
          <el-input v-model="ruleForm.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item label="别名：">
          <el-input v-model="ruleForm.alias" placeholder="请输入账号别名"></el-input>
        </el-form-item>
        <el-form-item label="公司标识：">
          <el-input v-model="ruleForm.firmLabel" placeholder="请设置公司标识，以英文加下划线组合"></el-input>
        </el-form-item>
        <el-form-item label="角色：">
          <el-checkbox-group v-model="checkedRole" @change="handleCheckedRoleChange">
            <el-checkbox v-for="city in cities" :label="city" :key="city">{{city}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        <el-form-item label="状态：">
          <el-radio-group v-model="checkedType" @change="handleCheckedTypeChange" v-for="item in radioNames"
                          :key="item.label">
            <el-radio :label="item.label">{{item.name}}</el-radio>
          </el-radio-group>
        </el-form-item>
        <h4>设备信息</h4>
        <el-row :gutter="40" style="margin-left: 20px;">
          <el-col :span="12">
            <el-card>
              <div slot="header">
                <el-row>
                  <el-col :span="2" style="line-height: 32px;">
                    <div>机型</div>
                  </el-col>
                  <el-col :span="16" style="margin-left:1em;">
                    <el-input placeholder="请输入机型名称" v-model="searchType" size="small" clearable></el-input>
                  </el-col>
                  <el-col :span="3">
                    <el-button type="primary" icon="el-icon-search" size="small">搜索</el-button>
                  </el-col>
                </el-row>
              </div>
              <div style="height: 200px;overflow-y: auto;">
                <el-checkbox v-for="o in 40" :key="o" style="margin: 5px 15px;">{{'机型' + o }}</el-checkbox>
              </div>
            </el-card>
          </el-col>
          <el-col :span="12">
            <el-card>
              <div slot="header">
                <el-row>
                  <el-col :span="3" style="line-height: 32px;">
                    <div>机器ID</div>
                  </el-col>
                  <el-col :span="15" style="margin-left:1em;">
                    <el-input placeholder="请输入机器ID" v-model="searchType" size="small" clearable></el-input>
                  </el-col>
                  <el-col :span="3">
                    <el-button type="primary" icon="el-icon-search" size="small">搜索</el-button>
                  </el-col>
                </el-row>
              </div>
              <div style="height: 200px;overflow-y: auto;">
                <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">全选
                </el-checkbox>
                <el-checkbox-group v-model="checkedRoboId" @change="handleCheckedRoboIdChange">
                  <el-checkbox v-for="roboId in robotIdList" :label="roboId" :key="roboId" style="margin: 5px 15px;">{{roboId}}</el-checkbox>
                </el-checkbox-group>
              </div>
            </el-card>
          </el-col>
        </el-row>
        <el-form-item style="text-align: center;margin-top: 2em;">
          <el-button type="primary" round size="medium" @click="handleNext('productInfoForm')"
                     style="padding: 10px 40px;">提交
          </el-button>
          <el-button round size="medium" @click="handlePrev" style="padding: 10px 40px;">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </el-card>
</template>

<script>
  import ElFormItem from "../../../../node_modules/element-ui/packages/form/src/form-item.vue";
  import ElCol from "element-ui/packages/col/src/col";

  export default {
    components: {
      ElCol,
      ElFormItem
    },
    name: "addUser",
    data() {
      return {
        ruleForm: {
          name: '',
          password: '',
          alias: '',
          firmLabel: ''
        },
        rules: {
          name: [
            {required: true, message: '请输入账号', trigger: 'blur'},
            {min: 6, max: 18, message: '长度为6~18个数字或字母组成', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            {min: 6, max: 16, message: '长度为6~16个数字或字母组成', trigger: 'blur'}
          ],
        },
        checkedRole: [],
        cities: ['角色1', '角色2', '角色3', '角色4'],
        radioNames: [
          {label: 0, name: '禁用'},
          {label: 1, name: '启用'}
        ],
        checkedType: 1,
        isIndeterminate: true,
        robotIdList: ['T1DL1555637394945', 'T2DL1555637394945', 'T3DL1555637394945', 'T4DL1555637394945'],
        checkAll: false,
        checkedRoboId: [],
        searchType: ''
      };
    },
    created() {
    },
    methods: {
      handleCheckAllChange(val) {
        this.checkedRoboId = val ? this.robotIdList : [];
        this.isIndeterminate = false;
      },
      handleCheckedRoboIdChange(value) {
        let checkedCount = value.length;
        this.checkAll = checkedCount === this.robotIdList.length;
        this.isIndeterminate = checkedCount > 0 && checkedCount < this.robotIdList.length;
      },

      handleNext(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$emit('nextStep');
          } else {
            this.$message({
              message: '验证失败',
              type: 'error',
              duration: 1000
            });
            return false;
          }
        });
      },
      handlePrev() {
        this.$router.push({path: '/sys/user'});
      },
      handleCheckedRoleChange(value) {
        console.log(value);
      },

      handleCheckedTypeChange(val) {
        console.log(val);
      }
    }
  }
</script>

<style scoped>
  .form-user {
    position: absolute;
    left: 0;
    right: 0;
    width: 60%;
    padding: 0px 35px 15px 35px;
    margin: 20px auto;
  }
</style>
