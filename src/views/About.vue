<template>
    <my-page title="关于">
        <div>带壳截图 v1.0.0</div>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
            }
        },
        mounted() {
            // this.$http.get('/version').then(
            //     response => {
            //         let data = response.data
            //         if (data.code === 200) {
            //             console.log(data)
            //             this.version = data.data
            //         }
            //     },
            //     response => {
            //         console.log(response)
            //     })
        }
    }
</script>

<style scoped>
</style>
