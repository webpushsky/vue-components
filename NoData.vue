<template>
    <div class="viscera-default">
        <div class="txt">{{ noData ? noData : '有效数据不足！'}}</div>
    </div>
</template>

<script>

export default {
    name: 'NoData',
    data () {
        return {
        }
    },
    props: {
        noData: String
    },
    mounted () {
    },
    created () {
        document.body.style.background = '#fff'
    },
    methods: {
    }
}
</script>

<style scoped>
    .viscera-default {
        position: absolute;
        margin-top: 200px;
        font-size: 26px;
        color: #898989;
        text-align: center;
        padding-bottom: 100px;
    }
    .viscera-default img {
        width: 80%;
    }
    .txt {
        position: absolute;
        top: 460px;
        left: 0;
        right: 0;
        margin: auto;
    }
</style>
