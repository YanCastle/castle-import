<template>
    <Row>
        <Col span="4">
            <input type="file" name="请选择导入文件" @change="loadFile">
        </Col>
        <Col span="4">
            <Button type="normal" @click="start">开始导入</Button>
            <Button type="normal" @click="downTemp">下载模板</Button>
        </Col>
        <Col>
            <table :class="classes">
                <thead>
                    <tr>
                        <td v-for="(v,k) in columns" :key="k">{{v.Title}}</td>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(v,k) in data" :key="k">
                        <td v-for="(o,p) in columns" :key="p">{{v[o.Column]}}</td>
                    </tr>
                </tbody>
            </table>
        </Col>
    </Row>
</template>
<script>
import { readAsJSON } from "castle-xlsx";
export default {
  props: {
    class: [Array, String],
    error: String,
    success: String,
    data: [Array, Object],
    columns: [Object]
  },
  data() {
    return {
      list: [],
      success: [],
      error: [],
      handing: -1,
      status: 0
    };
  },
  computed: {
    classes() {
      return this.class;
    }
  },
  mounted() {},
  methods: {
    start() {
      this.status = 1;
      //开始
      this.$emit("start");
    },
    pasue() {
      //暂停
      this.start = 2;
      this.$emit("pause");
    },
    downTemp() {
      this.$emit("down");
    },
    loadFile(e) {
      const files = e.target.files;
      if (!files) {
        return;
      }
      this.Loading = true;
      readAsJSON(files[0], d => {
        this.$emit("data", d);
        this.Loading = false;
      });
    }
  }
};
</script>
<style scoped>
</style>
