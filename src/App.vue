<template>
    <div id="app">
        <!-- <img src="./assets/logo.png"> -->
        <tinymce v-model="data" :uploadImage='uploadImage' style="min-height: 400px;"></tinymce>
        <button type="button" @click="clickHandler" style='margin-top: 20px;'>Reset</button>
        <div>{{ data }}</div>
    </div>
</template>

<script>
import tinymce from './components/TinymceVue'
import axios from 'axios'
import crypto from 'crypto'

const MD5 = (str) => {
    const hash = crypto.createHash('md5')
    hash.update(str)
    return hash.digest('hex').toString()
}

export default {
    name: 'app',
    components: {
        tinymce
    },
    data() {
        return {
            data: '',
            uploadImage: async (file) => {
                const getEliveToken = () => {
                    return new Promise((resolve, reject) => {
                        axios.get('http://10.0.60.95:7997/getEliveToken').then((data) => {
                            if (data.body.code === 1) {
                                resolve({ ts: data.body.data.ts, token: data.body.data.ltoken })
                            } else {
                                alert(data.body.msg + ', code:' + data.body.code)
                                reject(data.body.msg)
                            }
                        }).catch(function(data) {
                            if (data.body) {
                                alert(data.body.msg + ', code:' + data.body.code)
                                reject(data.body.msg)
                            } else {
                                alert('获取eliveToken失败, 网络异常')
                                reject('获取eliveToken失败, 网络异常')
                            }
                        })
                    })
                }
                const ts = (new Date()).getTime()
                const token = MD5('e02a4a4be3afd8899ff79898803d00cc' + ts)
                const uploadImg = () => {
                    return new Promise((resolve, reject) => {
                        const postData = new FormData()
                        postData.append('bizType', 5)
                        postData.append('file', file)
                        axios.post('http://10.0.60.95:8300/qiniu/image/single', postData, {
                            headers: {
                                'Content-Type': 'multipart/form-data',
                                'ts': ts.toString(),
                                'token': token
                            }
                        }).then(response => {
                            if (response.data.code === 1) {
                                resolve(response.data.data.url)
                            } else {
                                alert(response.data.msg)
                                reject(response.data.msg)
                            }
                        }, response => {
                            if (response.data.msg) {
                                alert(response.data.msg)
                                reject(response.data.msg)
                            } else {
                                alert('网络异常, 上传图片失败')
                                reject('网络异常, 上传图片失败')
                            }
                        })
                    })
                }
                return await uploadImg()
            }
        };
    },
    mounted() {

    },
    methods: {
        clickHandler() {
            this.data = '';
        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    /* margin-top: 60px; */
}
</style>
