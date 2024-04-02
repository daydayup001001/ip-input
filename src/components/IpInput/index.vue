<template>
  <div :class="getClassName">
    <div v-for="(val, i) in valueArr" :key="i" class="ip-input__item">
      <input
        ref="inputs"
        type="text"
        :value="isNaN(valueArr[i]) ? '' : valueArr[i]"
        @input="handleInput(i, $event)"
        @keydown="handleKeyDown(i, $event)"
      />
      <template v-if="i !== 3"><span>.</span></template>
    </div>
    <div v-if="isPort" class="ip-input__item">
      <template><span>:</span></template>
      <input
        ref="inputs"
        type="text"
        v-model="port"
        @input="handleInput(4, $event)"
        @keydown="handleKeyDown(4, $event)"
      />
    </div>
  </div>
</template>

<script>
import { getRange, isValidIPItemValue, isValidPortItemValue } from "./helps";

export default {
  props: {
    className: {
      type: String,
      default: "",
    },
    // 绑定值
    value: {
      type: String,
      default: "...",
    },
    isPort: {
      type: Boolean,
      default: false,
    },
    isError: {
      type: Function,
      default: () => false,
    },
    onChange: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      valueArr: [],
      port: "",
    };
  },
  mounted() {
    let { value, isPort } = this.$props;
    // 如果 value 不存在，则设置默认值

    if (!value) {
      value = isPort ? ".../" : "...";
    }
    if (isPort) {
      const ipPort = value.split("/");
      this.port = ipPort[1];
      value = ipPort[0].split(".");
    } else {
      value = value.split(".");
    }
    this.valueArr = value;
  },

  methods: {
    // 输入事件-i=4时为port
    handleInput(i, event) {
      let val = parseInt(event.target.value);

      if (i === 4) {
        // 限制字符串
        this.port = event.target.value.replace(/[^0-9]/g, "");

        // 限制范围
        if (
          event.target.value !== "" &&
          !isNaN(val) &&
          !isValidPortItemValue(val)
        ) {
          this.port = 65535;
        }
      } else {
        // 限制字符串
        this.$set(this.valueArr, i, event.target.value.replace(/[^0-9]/g, ""));

        // 限制范围
        if (
          event.target.value !== "" &&
          !isNaN(val) &&
          !isValidIPItemValue(val)
        ) {
          val = 255;
        }
        // 更新valueArr
        this.$set(this.valueArr, i, val);
        console.log("🚀 ~ this.valueArr:", this.valueArr);

        this.onPropsChange();
        if (!isNaN(val) && String(val).length === 3 && i < 3) {
          this.$refs.inputs[i + 1].focus();
        }
      }
      console.log("🚀 ~ this.getIpPortFormat:", this.getIpPortFormat);

      // emit更新
      this.$emit("input", this.getIpPortFormat);
      // this.$emit('change', newVal, oldVal);
    },

    // 按下键事件-i=4时为port
    handleKeyDown(i, event) {
      if (
        (event.keyCode === 37 || event.keyCode === 8) &&
        getRange(event.target).end === 0 &&
        i > 0
      ) {
        this.$refs.inputs[i - 1].focus();
      }
      if (
        event.keyCode === 39 &&
        getRange(event.target).end === event.target.value.length &&
        i < 3
      ) {
        this.$refs.inputs[i + 1].focus();
      }
      if (event.keyCode === 110 || event.keyCode === 190) {
        event.preventDefault();
        if (i < 3) {
          this.$refs.inputs[i + 1].focus();
        }
      }
    },

    onPropsChange() {
      this.onChange(this.getIpPortFormat);
    },
  },

  computed: {
    // 计算类名
    getClassName() {
      return [
        "ip-input",
        this.className,
        this.isError(this.valueArr.join(".")) ? "has-error" : "",
      ].join(" ");
    },

    // 计算ip-port标准格式
    getIpPortFormat() {
      const ipRes = this.valueArr
        .map((val) => (isNaN(val) ? "" : val))
        .join(".");
      const port = this.isPort ? `/${this.port}` : "";
      return `${ipRes}${port}`;
    },
  },
};
</script>

<style lang="scss" scoped>
.ip-input {
  display: inline-block;
  border: 1px solid #c2c3c9;
  border-radius: 4px;
  padding: 0 8px;
  background: #fff;
  height: 28px;
  line-height: 28px;

  span {
    color: #000;
    font-weight: 700;
    line-height: 28px;
  }
}

.ip-input .ip-input__item {
  display: inline-block;
}

.ip-input .has-error {
  border-color: #a94442;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}

.ip-input input {
  width: 46px;
  border: none;
  outline: none;
  text-align: center;
}
</style>
