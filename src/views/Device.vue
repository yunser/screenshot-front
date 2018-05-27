<template>
    <my-page class="page-device" title="带壳截图">
        <ul class="phone-list">
            <li class="item" v-for="phone in phones" @click="selectShell(phone)">
                <router-link class="link" :to="'/devices/' + phone.id">
                    <img class="thumb" src="/static/img/thumb.jpg">
                    <h4 class="name">{{ phone.name }}</h4>
                    <div class="size">{{ phone.size }}</div>
                </router-link>
            </li>
        </ul>
    </my-page>
</template>

<script>
    import devices from '@/data/data'

    export default {
        data () {
            return {
                phones: devices,
                phone: null,
                screenshot: '/static/img/screenshot.png'
            }
        },
        mounted() {
            this.phone = this.phones[0]
            // this.init()
        },
        methods: {
            selectShell(phone) {
                this.phone = phone
                this.init()
            },
            selectDevice() {
                this.$router.push('/devices')
            },
            fileChange(e) {
                let _this = this
                let files = e.target.files
                if (files.length > 0) {
                    console.log('啦啦')
                    let reader = new FileReader()
                    reader.onload = function (e) {
                        console.log('啦啦2212')
                        _this.resultSrc = this.result
                        _this.screenshot = this.result
                        _this.init()
                        // let img = new Image()
                        // img.onload = function () {
                        //     _this.originWidth = img.width
                        //     _this.originHeight = img.height
                        // }
                        // img.src = _this.resultSrc
                    }
                    reader.readAsDataURL(files[0])
                }
            },
            init() {
                let canvas = document.getElementById('canvas')
                let ctx = canvas.getContext('2d')
                canvas.style.width = '400px'
                // canvas.style.height = '100px'
                let shell = new Image()
                shell.onload = () => {
                    console.log(shell.width)
                    console.log(shell.height)
                    canvas.width = shell.width
                    canvas.height = shell.height
                    ctx.width = shell.width
                    ctx.height = shell.height
                    ctx.drawImage(shell, 0, 0)

                    let screenshotImg = new Image()
                    screenshotImg.onload = () => {
                        ctx.drawImage(screenshotImg, 0, 0, screenshotImg.width, screenshotImg.height,
                            this.phone.position[0], this.phone.position[1], this.phone.position[2], this.phone.position[3])
                    }
                    screenshotImg.src = this.screenshot
                }
                shell.src = this.phone.img
            }
        }
    }
</script>

<style lang="scss" scoped>
@import '../scss/var';

.phone-list {
    @include clearfix;
    .item {
        float: left;
        width: 140px;
        height: 160px;
        margin-right: 16px;
        margin-bottom: 16px;
        text-align: center;
        background-color: #fff;
        box-shadow: 0 1px 6px rgba(0,0,0,.117647), 0 1px 4px rgba(0,0,0,.117647);
    }
    .link {
        display: block;
        width: 100%;
        height: 100%;
        padding: 16px;
    }
    .thumb {
        width: 80px;
        height: 80px;
    }
    .name {
        color: #333;
        text-align: center;
    }
    .size {
        color: #9e9e9e;
    }
}
.btns {
    margin-bottom: 16px;
    .btn {
        margin-right: 8px;
    }
}
.ui-file-button{
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    opacity: 0;
}
.img {
    width: 300px;
}
canvas {
    // background-color: #f00;
    border: 1px solid #e9e9e9;
}
</style>
