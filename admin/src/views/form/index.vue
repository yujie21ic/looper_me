<template>
<div class="ipTab">
  <el-table :data="tableData" border style="width: 100%">
    <el-table-column type="index">
    </el-table-column>
    <el-table-column prop="ip" label="ip地址">
    </el-table-column>
    <el-table-column prop="address" label="操作">
      <template slot-scope="scope">
        <el-button size="mini" type="danger" @click="prohibition(scope.row)">封禁</el-button>
        <el-button size="mini" type="success" @click="pullBlack(scope.row)">拉黑</el-button>
      </template>
    </el-table-column>
  </el-table>
</div>
</template>

<script>
export default {
  inject:['reload'],
  data() {
    return {
      ipURL: this.URL + '/ip_manage',
      tableData: []
    }
  },

  methods: {
    getIpData() {
      this.axios.get(this.ipURL).then(data => {
        let returnData = data.data
        if (returnData.success == true) {
          this.tableData = returnData.data
        }
      }).catch(err => {
        console.log(err);
      })
    },
    prohibition(row) {
      let sendData = {
        'todo': 'kill',
        'ip': row.ip
      }
      this.axios.post(this.ipURL, this.$qs.stringify(sendData)).then(data => {
        console.log(data)
        let returnData = data.data
        console.log(returnData.success)
        if (returnData.success == true) {
          this.$message({
            message: returnData.msg,
            type: 'success',
            center: true
          });
          setTimeout(()=>{
            this.reload()
          },1500)

        }
      }).catch(err => {
        console.log(err)
      })
    },
    pullBlack(row) {
      let sendData = {
        'todo': 'pull_black',
        'ip': row.ip
      }
      this.axios.post(this.ipURL, this.$qs.stringify(sendData)).then(data => {
        let returnData = data.data
        if (returnData.success == true) {
          this.$message({
            message: returnData.msg,
            type: 'success',
            center: true
          });
          setTimeout(()=>{
            this.reload()
          },1500)
        }
      }).catch(err => {
        console.log(err);
      })
    }
  },

  mounted() {
    this.getIpData()
  }
}
</script>

<style scoped>
.ipTab {
  padding: 20px;
}
</style>
