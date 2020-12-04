<template>
  <div>
    <el-form>
      <el-form-item>
        <el-button type="warning" size="small" @click="handleImport">导入交易数据</el-button>
      </el-form-item>
    </el-form>
    <input type="file" ref="filElem" class="file" @change="getFile" />
  </div>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    // 导入
    handleImport() {
      this.$refs.filElem.dispatchEvent(new MouseEvent('click'))
    },
    getFile() {
      var that = this
      // 获取文件
      const inputFile = this.$refs.filElem.files[0]
      if (inputFile) {
        // 判断文件格式为表格
        let fileName = inputFile.name.split('.')
        if (fileName[fileName.length - 1] === 'xlsx' || fileName[fileName.length - 1] === 'xls' || fileName[fileName.length - 1] === 'xltx' || fileName[fileName.length - 1] === 'xlt' || fileName[fileName.length - 1] === 'xlsm' || fileName[fileName.length - 1] === 'xlsb' || fileName[fileName.length - 1] === 'xltm' || fileName[fileName.length - 1] === 'csv') {
          // 组装参数-formdata
          let formData = new window.FormData()
          formData.append('file', inputFile)
          let options = {
            // 设置axios的参数
            url: `${process.env.VUE_APP_BASE_API}/customInfo/upload`,
            data: formData,
            method: 'post',
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
          const loading = this.$loading({
            lock: true,
            text: 'Loading',
            spinner: 'el-icon-loading',
            background: 'rgba(0, 0, 0, 0.7)'
          })
          // 请求接口-并配置请求头
          axios(options).then(res => {
            if (res.data.code == '0000') {
              loading.close()
              this.$message.success(res.data.msg)
            }
            loading.close()
          })
        } else {
          this.$message.error('请上传表格文件')
          return
        }
      } else {
        return
      }
    }
  }
}
</script>

<style>
.file {
  visibility: hidden;
}
</style>
