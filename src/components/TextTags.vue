<template>
    <div :class="['text-tags', alignmentClass]">
      <div
        class="text-tags__tag"
        v-for="(tag, index) in visibleTags"
        :key="index"
        :class="{
          'text-tags__tag--icon': tag.icon,
          'text-tags__tag--last': index === visibleTags.length - 1
        }"
      >
        <span>{{ tag.text }}</span>
        <v-icon v-if="tag.icon" class="text-tags__icon">{{ tag.icon }}</v-icon>
        <v-icon v-if="index < visibleTags.length - 1" class="text-tags__separator">mdi-circle-small</v-icon>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TextTags',
    props: {
      tags: {
        type: Array,
        required: true
      },
      alignment: {
        type: String,
        default: 'left',
        validator: value => ['left', 'justify'].includes(value)
      }
    },
    data() {
      return {
        visibleTags: []
      };
    },
    computed: {
      alignmentClass() {
        return this.alignment === 'justify' ? 'text-tags--justify' : 'text-tags--left';
      }
    },
    mounted() {
      this.updateVisibleTags();
      window.addEventListener('resize', this.updateVisibleTags);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.updateVisibleTags);
    },
    methods: {
      updateVisibleTags() {
        this.$nextTick(() => {
          const containerWidth = this.$el.clientWidth;
          let totalWidth = 0;
          this.visibleTags = [];
          for (const tag of this.tags) {
            const tagElement = document.createElement('span');
            tagElement.innerText = tag.text;
            tagElement.style.visibility = 'hidden';
            document.body.appendChild(tagElement);
            const tagWidth = tagElement.offsetWidth + (tag.icon ? 24 : 0) + 24; // Adjust this value as needed
            document.body.removeChild(tagElement);
            if (totalWidth + tagWidth > containerWidth) {
              break;
            }
            totalWidth += tagWidth;
            this.visibleTags.push(tag);
          }
        });
      }
    }
  };
  </script>
  
  <style scoped lang="scss">
  .text-tags {
    display: flex;
    flex-wrap: nowrap;
    align-items: center;
    overflow: hidden;
  
    &--left {
      justify-content: flex-start;
    }
  
    &--justify {
      justify-content: space-between;
    }
  
    &__tag {
      display: flex;
      align-items: center;
      padding-right: 12px; // Adjust spacing between tag elements as needed
  
      &--icon {
        margin-right: 8px; // Adjust spacing between text and icon as needed
      }
  
      &--last .text-tags__separator {
        display: none;
      }
    }
  
    &__separator {
      margin: 0 12px; // Adjust spacing between tags and separators as needed
    }
  }
  
  .text-tags__tag {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  </style>
  