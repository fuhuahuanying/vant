<template>
  <demo-section>
    <van-tabs v-model="tab" sticky :color="BLUE">
      <van-tab :title="$t('basic')">
        <van-col v-for="icon in icons.basic" :key="icon" span="6" @click="copy(icon)">
          <van-icon :name="icon" />
          <span>{{ icon }}</span>
        </van-col>
      </van-tab>

      <van-tab :title="$t('outline')">
        <van-col v-for="icon in icons.outline" :key="icon" span="6" @click="copy(icon)">
          <van-icon :name="icon" />
          <span>{{ icon }}</span>
        </van-col>
      </van-tab>

      <van-tab :title="$t('filled')">
        <van-col v-for="icon in icons.filled" :key="icon" span="6" @click="copy(icon)">
          <van-icon :name="icon" />
          <span>{{ icon }}</span>
        </van-col>
      </van-tab>

      <van-tab :title="$t('demo')">
        <demo-block :title="$t('dot')">
          <van-col span="6">
            <van-icon :name="demoIcon" @click="copy(demoIcon)" />
            <span>{{ demoIcon }}</span>
          </van-col>
          <van-col span="6">
            <van-icon :name="demoIcon" dot @click="copy(demoIcon, { dot: true })" />
            <span>{{ demoIcon }}</span>
          </van-col>
        </demo-block>
        <demo-block :title="$t('message')">
          <van-col span="6">
            <van-icon :name="demoIcon" @click="copy(demoIcon)" />
            <span>{{ demoIcon }}</span>
          </van-col>
          <van-col span="6">
            <van-icon :name="demoIcon" info="123" @click="copy(demoIcon, { info: '123' })" />
            <span>{{ demoIcon }}</span>
          </van-col>
        </demo-block>
        <demo-block :title="$t('color')">
          <van-col span="6">
            <van-icon :name="demoIcon" @click="copy(demoIcon)" />
            <span>{{ demoIcon }}</span>
          </van-col>
          <van-col span="6">
            <van-icon :name="demoIcon" color="#ff0000" @click="copy(demoIcon, { color: '#ff0000' })" />
            <span>{{ demoIcon }}</span>
          </van-col>
        </demo-block>
        <demo-block :title="$t('size')">
          <van-col span="6">
            <van-icon :name="demoIcon" @click="copy(demoIcon)" />
            <span>{{ demoIcon }}</span>
          </van-col>
          <van-col span="6">
            <van-icon :name="demoIcon" size="40px" @click="copy(demoIcon, { size: '40px' })" />
            <span>{{ demoIcon }}</span>
          </van-col>
        </demo-block>
      </van-tab>
    </van-tabs>
  </demo-section>
</template>

<script>
import icons from '@vant/icons';
import { BLUE } from '../../utils/constant';

// from https://30secondsofcode.org
function copyToClipboard(str) {
  const el = document.createElement('textarea');
  el.value = str;
  el.setAttribute('readonly', '');
  el.style.position = 'absolute';
  el.style.left = '-9999px';
  document.body.appendChild(el);

  const selected =
    document.getSelection().rangeCount > 0
      ? document.getSelection().getRangeAt(0)
      : false;

  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);

  if (selected) {
    document.getSelection().removeAllRanges();
    document.getSelection().addRange(selected);
  }
}

export default {
  i18n: {
    'zh-CN': {
      title: '图标列表',
      info: '提示信息',
      basic: '基础图标',
      copied: '复制成功',
      outline: '线框风格',
      filled: '实底风格',
      demo: '用法示例',
      dot: '图标右上角小红点',
      message: '图标右上角徽标的内容',
      color: '图标颜色',
      size: '图标大小'
    },
    'en-US': {
      title: 'Icon List',
      info: 'Show Info',
      basic: 'Basic',
      copied: 'Copied',
      outline: 'Outline',
      filled: 'Filled',
      demo: 'Demo',
      dot: 'Show Red Dot',
      message: 'Show Info',
      color: 'Icon Color',
      size: 'Icon Size'
    }
  },

  data() {
    this.BLUE = BLUE;
    this.icons = icons;
    return {
      tab: 0,
      demoIcon: 'location-o'
    };
  },

  methods: {
    copy(icon, option = {}) {
      let tag = `<van-icon name="${icon}"`;
      if ('dot' in option) {
        tag = `${tag} ${option.dot ? 'dot' : ''}`;
      }
      if ('info' in option) {
        tag = `${tag} info="${option.info}"`;
      }
      if ('color' in option) {
        tag = `${tag} color="${option.color}"`;
      }
      if ('size' in option) {
        tag = `${tag} size="${option.size}"`;
      }
      tag = `${tag} />`;
      copyToClipboard(tag);

      this.$notify({
        type: 'success',
        duration: 1500,
        className: 'demo-icon-notify',
        message: `${this.$t('copied')}：${tag}`
      });
    }
  }
};
</script>

<style lang="less">
@import '../../style/var';

.demo-icon {
  font-size: 0;

  &-list {
    box-sizing: border-box;
    min-height: calc(100vh - 65px);
    padding-top: 10px;
  }

  &-notify {
    font-size: 13px;
  }

  .van-col {
    display: inline-block;
    float: none;
    height: 100px;
    text-align: center;
    vertical-align: middle;

    span {
      display: block;
      padding: 0 5px;
      color: @gray-7;
      font-size: 12px;
      line-height: 18px;
    }

    &:active {
      background-color: @active-color;
    }
  }

  .van-icon {
    margin: 20px 0 10px;
    color: @text-color;
    font-size: 32px;
  }

  .van-tab__pane {
    width: auto;
    margin: 20px;
    background-color: #fff;
  }
}
</style>
