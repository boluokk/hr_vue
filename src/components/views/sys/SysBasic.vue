<template>
  <div>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>基本资料设置</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card shadow="always">
      <el-tabs
        v-model="activeName"
        type="border-card"
        @tab-click="tablesHandel"
      >
        <el-tab-pane label="部门管理" name="mana"
          ><ManaComp :manageData="manageData" :initMana="initMana"></ManaComp
        ></el-tab-pane>
        <el-tab-pane label="职位管理" name="job"
          ><JobComp :position="position" :initPos="initPos"></JobComp
        ></el-tab-pane>
        <el-tab-pane label="职称管理" name="jobname"
          ><JobNameComp
            :jobNametableData="jobNametableData"
            :initJobName="initJobName"
          ></JobNameComp
        ></el-tab-pane>
        <el-tab-pane label="奖惩规则" name="reward"
          ><RewardComp></RewardComp
        ></el-tab-pane>
        <el-tab-pane label="权限组" name="rights"
          ><RightsComp
            :rightsData="rightsData"
            :rightsData2="rightsData2"
            :initRights="initRights"
          ></RightsComp
        ></el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<script>
import ManaComp from './sysBasicCompoents/ManaComp.vue'
import JobComp from './sysBasicCompoents/JobComp.vue'
import JobNameComp from './sysBasicCompoents/JobNameComp.vue'
import RewardComp from './sysBasicCompoents/RewardComp.vue'
import RightsComp from './sysBasicCompoents/RightsComp.vue'

export default {
  data() {
    return {
      activeName: 'job',
      position: [],
      jobNametableData: [],
      rightsData: [],
      rightsData2: [],
      manageData: []
    }
  },
  components: {
    ManaComp,
    JobComp,
    JobNameComp,
    RewardComp,
    RightsComp
  },
  methods: {
    tablesHandel(tab) {
      if (tab.name === 'job') {
        this.initPos()
      } else if (tab.name === 'jobname') {
        this.initJobName()
      } else if (tab.name === 'rights') {
        this.initRights()
      } else if (tab.name === 'mana') {
        this.initMana()
      }
    },
    initPos() {
      this.getRequest('/system/basic/job/').then(res => {
        this.position = res.data.obj
      })
    },
    initJobName() {
      this.getRequest('/system/basic/jobname/').then(res => {
        this.jobNametableData = res.data.obj
      })
    },
    async initRights() {
      this.getRequest('/system/basic/rights/').then(res => {
        this.rightsData = res.data.obj
      })
      this.getRequest('/system/basic/rights/menus').then(res => {
        this.rightsData2 = res.data.obj
      })
    },
    initMana() {
      this.getRequest('/system/basic/department/').then(res => {
        this.manageData = res.data.obj
      })
    }
  },
  created() {
    this.initPos()
  }
}
</script>

<style lang="less" scoped>
.el-card {
  margin-top: 15px;
}
</style>
