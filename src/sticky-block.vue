<template>
    <div>
        <div ref="placeholder"
             v-bind:style="placeholderStyles"
        ></div>
        <div ref="sticky"
             v-bind:style="stickyStyles"
        >
            <slot></slot>
        </div>
    </div>
</template>
<script>
  /**
   *
   */
  export default {
    props: {
      topOffset: {
        type: Number,
        default: null,
      },
      bottomOffset: {
        type: Number,
        default: null,
      },
      bottomPosition: {
        type: Number,
        default: null,
      },
      /**
       * HTMLElement
       */
      container: {
        default: null,
      },
      containerTopOffset: {
        type: Number,
        default: 0,
      },
      containerBottomOffset: {
        type: Number,
        default: 0,
      },
    },
    data() {
      return {
        height: 0,
        width: 0,
        top: 'auto',
        right: 'auto',
        left: 'auto',
        isPending: false,
      };
    },
    computed: {
      isStick() {
        return this.top !== 'auto';
      },
      stickyStyles() {
        return {
          position: (this.isStick === true) ? 'fixed' : 'static',
          top: (this.isStick === true) ? `${this.top}px` : '0px',
          left: (this.isStick === true) ? `${this.left}px` : '0px',
          width: `${this.width}px`,
        };
      },
      placeholderStyles() {
        const height = this.isStick ? this.height : 0;
        return {
          paddingTop: `${height}px`,
        };
      }
    },
    mounted() {
      this.update();
      ['load', 'resize', 'scroll'].forEach((eventName) => {
        window.addEventListener(eventName, () => this.requestUpdate(), {passive: true});
      });
    },
    methods: {
      requestUpdate() {
        if (this.isPending === true) {
          return;
        }

        this.isPending = true;
        requestAnimationFrame(() => {
          this.isPending = false;
          this.update();
        });
      },
      update() {
        const placeholder = this.$refs.placeholder.getBoundingClientRect();
        const sticky = this.$refs.sticky.getBoundingClientRect();

        this.top = 'auto';
        this.left = placeholder.left;
        this.width = placeholder.width;
        this.height = sticky.height;

        if (this.topOffset !== null
          && placeholder.top < this.topOffset
        ) {
          this.top = this.topOffset;
        }

        if (this.bottomPosition !== null) {
          const topLimit = window.innerHeight
            - this.bottomPosition
            - this.height;

          if (placeholder.top < topLimit) {
            this.top = topLimit;
          }
        }

        const bottomLimit = window.innerHeight
          - this.bottomOffset
          - this.height;

        if (this.bottomOffset !== null
          && placeholder.top > bottomLimit
        ) {
          this.top = bottomLimit;
        }

        if (this.container === null
          || this.top === 'auto'
        ) {
          return;
        }

        this.updateContainerLimits();
      },
      updateContainerLimits() {
        const container = this.container.getBoundingClientRect();
        const topContainerLimit = container.bottom
          - this.height
          - this.containerBottomOffset;

        if (this.top >= topContainerLimit) {
          this.top = topContainerLimit;
        }

        const bottomContainerLimit = container.top
          + this.containerTopOffset;

        if (this.top <= bottomContainerLimit) {
          this.top = bottomContainerLimit;
        }
      }
    },
  };
</script>