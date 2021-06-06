<template>
  <div class="barecodes">
    <el-row>
      <el-col :span="6"
        ><div class="grid-content bg-purple">
          <el-link type="primary" :underline="false" href="#"
            >鼠标点击框外自动生成条形码
          </el-link>
        </div></el-col
      >
      <el-col :span="12"
        ><div class="grid-content bg-purple-light">
          <el-input
            type="textarea"
            :autosize="{ minRows: 4, maxRows: 8 }"
            placeholder="将运单号复制到这里"
            v-model="text"
            @blur="textAreaOnBlur"
            resize="false"
            clearable
          >
          </el-input>
        </div>
      </el-col>
      <el-col :span="6">
        <div class="grid-content bg-purple"></div>
        共<el-link type="primary" :underline="false" href="#">{{
          listSize
        }}</el-link
        >条数据</el-col
      >
    </el-row>
    <el-divider></el-divider>
    <el-row>
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="20"><div id="codes"></div> </el-col>
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import jsbarcode from "jsbarcode";
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return { listSize: 0, strList: {}, text: "" };
  },
  created() {},
  mounted() {},
  methods: {
    textAreaOnBlur() {
      this.strList = this.text.split(/[\s\n]/);
      console.info(this.strList);
      var codeRoot = document.getElementById("codes");
      codeRoot.innerHTML = "";
      this.listSize = 0;
      try {
        for (var i = 0; i < this.strList.length; i++) {
          if (this.strList[i].length > 2) {
            codeRoot.innerHTML =
              codeRoot.innerHTML + '<svg id="' + this.strList[i] + '"></svg>';
            jsbarcode("#" + this.strList[i], this.strList[i]);
            this.listSize += 1;
          }
        }
      } catch (e) {
        this.$message.error("输入内容有误");
      } finally {
        console.info();
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#codes {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  svg {
    margin: 10px;
  }
}
.el-link {
  font-size: 20px;
}
</style>
