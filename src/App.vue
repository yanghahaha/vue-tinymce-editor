<template>
    <div id="app">
        <tinymce ref="mce" v-model="data" :uploadImageHandle='uploadImage' style="min-height: 400px;"></tinymce>
        <button type="button" @click="clickHandler" style='margin-top: 20px;'>重置编辑器内容</button>
        <button type="button" @click="removeEditor" style='margin-top: 20px;'>移除编辑器</button>
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
            uploadImage: (file) => {
                return new Promise((resolve, reject) => {
                    const postData = new FormData()
                    postData.append('file', file)
                    axios.post('yoururl', postData, {
                        headers: {
                            'Content-Type': 'multipart/form-data',
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
        };
    },
    mounted() {
        // this.removeEditor()
    },
    methods: {
        clickHandler() {
            this.data = '';
        },
        removeEditor() {
            this.$refs.mce.$destroy()
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
