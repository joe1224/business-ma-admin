<template>
  <el-card class="form-container" shadow="never">
    <h4>基础信息</h4>
    <div style="margin-top: 50px">
      <el-form :model="ruleForm" :rules="rules" ref="productInfoForm" label-width="120px" style="width: 600px"
               size="small">
        <el-form-item label="账号：" prop="name">
          <el-input v-model="ruleForm.name" placeholder="请输入账号"></el-input>
        </el-form-item>
        <el-form-item label="密码：" prop="password">
          <el-input v-model="ruleForm.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item label="商家类型：">
          <el-select placeholder="请选择" v-model="listQuery.typeOption" style="width:100%;">
            <el-option v-for="item in type" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item  label="角色：">
          <el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange">
            <el-checkbox v-for="city in cities" :label="city" :key="city">{{city}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        <el-form-item  label="状态：">
          <el-radio-group v-model="checkedType" @change="handleCheckedTypeChange" v-for="item in radioNames" :key="item.label">
            <el-radio :label="item.label">{{item.name}}</el-radio>
          </el-radio-group>
        </el-form-item>
        <h4>设备信息</h4>

        <el-form-item style="text-align: center">
          <el-button type="primary" size="medium" @click="handleNext('productInfoForm')">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </el-card>
</template>

<script>
  const defaultListQuery = {
    typeOption:''
  };
  export default {
    name: "addUser",
    data() {
      return {
        listQuery: Object.assign({}, defaultListQuery),
        type:[
          {value: 0,label: '勇艺达'},
          {value: 1,label: '玖富'}
        ],
        ruleForm:{
          name:'',
          password:''
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
        checkedCities: [],
        cities: ['角色1', '角色2', '角色3', '角色4'],
        radioNames:[
          {label: 0,name: '禁用'},
          {label: 1,name: '启用'}
        ],
        checkedType:''
      };
    },
    created() {
    },
    methods: {
      handleNext(formName){
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$emit('nextStep');
          } else {
            this.$message({
              message: '验证失败',
              type: 'error',
              duration:1000
            });
            return false;
          }
        });
      },

      handleCheckedCitiesChange(value) {
        console.log(value);
      },

      handleCheckedTypeChange(val){
        console.log(val);
      }
    }
  }
</script>

<style scoped>
</style>
