<template>
    <my-page :title="title" backable>
        <div class="btns">
            <ui-raised-button class="btn file-select-btn" label="上传截图" primary>
                <input type="file" class="ui-file-button" accept="image/*" @change="fileChange($event)">
            </ui-raised-button>
            <!-- <ui-raised-button class="btn" label="选择设备" @click="selectDevice" /> -->
        </div>
        
        <canvas id="canvas"></canvas>
    </my-page>
</template>

<script>
    import devices from '@/data/data'

    export default {
        data () {
            return {
                title: '带壳截图',
                phones: devices,
                phone: null,
                screenshot: '/static/img/screenshot.png'
            }
        },
        mounted() {
            let deviceId = this.$route.params.id
            console.log(deviceId)
            for (let device of devices) {
                if (device.id === deviceId) {
                    this.phone = device
                    this.title = this.phone.name
                    this.init()
                    break
                }
            }
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
    max-width: 100%;
    // background-color: #f00;
    border: 1px solid #e9e9e9;
}
</style>
