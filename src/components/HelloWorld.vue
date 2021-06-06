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
          resultList.length
        }}</el-link
        >条数据</el-col
      >
    </el-row>
    <el-divider></el-divider>
    <el-row class="codeAll">
      <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="16">
        <div class="block">
          <span class="demonstration"></span>
          <el-carousel
            id="carouselId"
            trigger="click"
            height="200px"
            :autoplay="false"
          >
            <el-carousel-item v-for="item in codeList" :key="item">
              <svg
                :class="item"
                jsbarcode-format="CODE128"
                :jsbarcode-value="item"
              ></svg>
            </el-carousel-item>
          </el-carousel>
        </div>
      </el-col>
      <el-col :span="4"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-divider></el-divider>
    <el-row class="codeAll">
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="20"><div id="codes"></div> </el-col>
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import JsBarcode from "jsbarcode";
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return { strList: [], resultList: [], text: "" };
  },
  created() {
    console.info("created");
  },
  updated() {
    for (var i = 0; i < this.resultList.length; i++) {
      JsBarcode("." + this.resultList[i]).init();
    }
    this.$nextTick(function () {
      for (var i = 0; i < this.resultList.length; i++) {
        JsBarcode("." + this.resultList[i]).init();
      }
    });
    console.info("updated");
  },
  computed: {
    codeList() {
      return this.resultList;
    },
  },
  methods: {
    textAreaOnBlur() {
      this.strList = this.text.split(/[\s\n]/);
      this.resultList = [];
      console.info(this.strList);
      var codeRoot = document.getElementById("codes");
      codeRoot.innerHTML = "";
      try {
        for (var i = 0; i < this.strList.length; i++) {
          if (this.strList[i].length > 1) {
            codeRoot.innerHTML =
              codeRoot.innerHTML +
              '<div><svg id="' +
              this.strList[i] +
              '"></svg></div>';

            JsBarcode("#" + this.strList[i], this.strList[i]);
            this.listSize += 1;
            this.resultList.push(this.strList[i]);
          }
        }
        this.$nextTick(function () {
          for (var i = 0; i < this.resultList.length; i++) {
            JsBarcode("." + this.resultList[i]).init();
          }
        });
      } catch (e) {
        console.info(e);
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
  width: 100%;
  margin: 30px;
  div {
    float: left;
    margin: 20px;
  }
}
.el-link {
  font-size: 20px;
}

.codeAll {
  margin-top: 80px;
}
</style>
