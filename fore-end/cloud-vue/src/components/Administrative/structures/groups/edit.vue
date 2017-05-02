<template>
  <div class="m-l-50 m-t-30 w-900">
    <el-form ref="form" :model="form" :rules="rules" label-width="130px">
      <el-form-item label="用户组名称" prop="title">
        <el-input v-model.trim="form.title" class="h-40 w-200"></el-input>
      </el-form-item>
      <el-form-item label="父级用户组" prop="pid">
        <el-select v-model="form.pid" placeholder="父级用户组" class="w-200">
          <el-option v-for="item in options" :label="item.title" :value="item.id"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="备注">
        <el-input
          type="textarea"
          :rows="2"
          placeholder="备注"
          v-model="form.remark"
          class="w-200">
        </el-input>
      </el-form-item>
      <el-form-item label="权限分配">
       <!-- <div class="bor-gray h-400 ovf-y-auto bor-ra-5 bg-wh">
          <div v-for="item in nodes">
            <div class="bor-b-ccc bg-gra p-l-10 p-r-10">
              <el-checkbox v-model="item.check" @change="selectProjectRule(item)">{{item.title}}</el-checkbox>
            </div>
            <div v-for="childItem in item.child">
              <div class="p-l-20 bor-b-ccc">
                <el-checkbox v-model="childItem.check" @change="selectModuleRule(childItem, item, childItem.child)">{{childItem.title}}</el-checkbox>
              </div>
              <div class="p-l-40 bor-b-ccc bg-gra">
                <el-checkbox v-for="grandChildItem in childItem.child" v-model="grandChildItem.check" @change="selectActionRule(grandChildItem, childItem, item)">{{grandChildItem.title}}</el-checkbox>
              </div>
            </div>
          </div>
        </div>-->
        <el-tree
                :data="nodes"
                show-checkbox
                default-expand-all
                node-key="id"
                ref="tree"
                highlight-current
                :props="defaultProps">
        </el-tree>

      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="edit('form')" :loading="isLoading">提交</el-button>
        <el-button @click="goback()">返回</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
  import http from '../../../../assets/js/http'
  import fomrMixin from '../../../../assets/js/form_com'

  export default {
    data() {
      return {
        isLoading: false,
        form: {
          id: null,
          title: '',
          pid: '',
          remark: '',
          rules: '',
          status: null
        },
        options: [{ pid: '0', title: '无' }],
        nodes: [],
        selectedNodes: [],
        rules: {
          title: [
            { required: true, message: '请输入用户组名称', trigger: 'blur' }
          ]
        },
        defaultProps: {
          children: 'child',
          label: 'title'
        }
      }
    },
    methods: {
      edit(form) {
        this.form.rules = this.$refs.tree.getCheckedKeys().toString()
        this.$refs[form].validate((valid) => {
          if (valid) {
            this.isLoading = !this.isLoading
            this.apiPost('admin/groups/update', this.form).then((res) => {
              this.handelResponse(res, (data) => {
                _g.toastMsg('success', '编辑成功')
                setTimeout(() => {
                  this.goback()
                }, 1500)
              }, () => {
                this.isLoading = !this.isLoading
              })
            })
          }
        })
      },
      getRules() {
        return new Promise((resolve, reject) => {
          this.apiGet('admin/rules?type=tree').then((res) => {
            this.handelResponse(res, (data) => {
              resolve(data)
            })
          })
        })
      },
      getGroups() {
        this.apiGet('admin/groups').then((res) => {
          console.log('res = ', _g.j2s(res))
          this.handelResponse(res, (data) => {
            _(data).forEach((ret) => {
              ret.id = ret.id.toString()
            })
            this.options = this.options.concat(data)
          })
        })
      },
      async getGroupInfo() {
        this.form.id = this.$route.params.id
        let arr = await this.getRules()
        this.nodes = this.nodes.concat(arr)
        this.apiGet('admin/groups/edit/' + this.form.id).then((res) => {
          this.handelResponse(res, (data) => {
            this.form.title = data.title
            this.form.pid = data.pid ? data.pid.toString() : ''
            this.form.remark = data.remark
            this.form.status = data.status
            let array = data.rules.split(',')
            this.$refs.tree.setCheckedKeys(array)
          })
        })
      }
    },
    created() {
      this.getGroupInfo()
      this.getGroups()
    },
    mixins: [http, fomrMixin]
  }
</script>