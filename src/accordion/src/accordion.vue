<template lang="html">
  <div :class="prefixCls">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'zaAccordion',
  props: {
    prefixCls: {
      type: String,
      default: 'za-accordion',
    },
    defaultActiveTag: {
      type: Array,
      default: () => [],
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    animated: {
      type: Boolean,
      default: false,
    },
    open: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      activeTag: [],
    };
  },
  created() {
    this.activeTag = this.getActiveIndex();
  },
  methods: {
    onItemChange(key) {
      const { multiple, activeTag } = this;
      const hasKey = activeTag.indexOf(key) > -1;

      let newActiveIndex = [];
      if (!multiple) {
        if (hasKey) {
          newActiveIndex = activeTag.filter(i => i !== key);
        } else {
          newActiveIndex = activeTag.slice(0);
          newActiveIndex.push(key);
        }
      } else {
        newActiveIndex = hasKey ? [] : [key];
      }
      this.activeTag = newActiveIndex;
      this.$emit('change', key);
    },
    getActiveIndex() {
      const { defaultActiveTag, multiple } = this;
      const defaultIndex = (defaultActiveTag !== undefined) ? defaultActiveTag : [];
      return multiple ? [defaultIndex[0]] : defaultIndex;
    },
  },
};
</script>
