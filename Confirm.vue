<template>
    <div>
        <div class="anx-confirm-bgs" v-if="showValue">
        </div>
        <div :style="{width: confirmWidth + 'px'}" class="anx-confirm-boxs" v-if="showValue">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AnxConfirm',
    data () {
        return {
            showValue: false
        }
    },
    props: {
        value: {
            type: Boolean,
            default: false
        },
        confirmWidth: {
            type: Number,
            default: 0
        }
    },
    watch: {
        value (val) {
            this.showValue = val
            if (val === true) {
                document.documentElement.style.overflow = 'hidden'
                document.body.style.overflow = 'hidden'
            } else {
                document.documentElement.style.overflow = ''
                document.body.style.overflow = ''
            }
        }
    },
    methods: {
    },
    created () {
        this.showValue = this.value
        if (this.value) {
            document.documentElement.style.overflow = 'hidden'
            document.body.style.overflow = 'hidden'
            this.showValue = this.value
        }
    }
}
</script>

<style scoped lang="stylus" type="text/stylus">
.anx-confirm-bgs {
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  z-index: 9999;
  width: 100%;
  height: 100%;
  overflow: auto;
  background: #000;
  opacity: 0.5;
}
.anx-confirm-boxs {
  position: fixed;
  left:50%;
  top:45%;
  transform:translateX(-50%) translateY(-50%);
  padding: 30px 36px 0 24px;
  background: #fff;
  border-radius: 9px;
  z-index: 99999;
  color: #666;
  font-size: 14px;
  text-align: center;
}
</style>
