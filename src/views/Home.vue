<template>
    <my-page title="带壳截图">
        <ul>
            <li v-for="phone in phones" @click="selectShell(phone)">{{ phone.name }}</li>
        </ul>
        <ui-raised-button class="file-select-btn" label="选择图片" primary>
            <input type="file" class="ui-file-button" accept="image/*" @change="fileChange($event)">
        </ui-raised-button>
        <canvas id="canvas"></canvas>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                phones: [
                    {
                        name: '通用',
                        img: 'http://img1.yunser.com/screenshot/ke.png',
                        position: [263, 482, 1076, 1922]
                    },
                    {
                        name: '小米',
                        img: 'http://img1.yunser.com/screenshot/mi.png',
                        position: [196, 296, 713, 1284]
                    },
                    {
                        name: '三星 Note4 白色',
                        img: 'http://img1.yunser.com/screenshot/samsung_note4_white.png',
                        position: [176, 320, 732, 1290]
                    }
                ],
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
