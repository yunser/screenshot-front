<template>
    <my-page title="带壳截图">
        <canvas id="canvas"></canvas>
        <img class="img" src="/static/img/mi.png">
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                let canvas = document.getElementById('canvas')
                let ctx = canvas.getContext('2d')
                canvas.style.width = '100px'
                canvas.style.height = '100px'
                ctx.width = 100
                ctx.height = 100
                ctx.fillRect(0, 0, 20, 20)
            }
        }
    }
</script>

<style lang="scss" scoped>
.img {
    width: 300px;
}
canvas {
    background-color: #f00;
}
</style>
