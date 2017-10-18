# vue-tinymce-zh_ch

## 一. 前言

    1. 本组件是对tinymce的封装,汉化及二次开发.
    2. 语言包下载地址: https://www.tinymce.com/download/language-packages/
    3. 考虑到上传图片总是涉及鉴权等业务逻辑, 上传方法必须由外部传入.
        a. 如果不配置uploadImage方法, 将不会有选择图片按钮, 只能手动输入图片url.
        b. 参数: file, 返回: Promise, resolve(url)
        d. 示例(需按实际业务调整):
            uploadImageHandle: (file) => {
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
        4. 视频上传功能的业务更复杂, 暂时不考虑

## 二. 参数说明:

        value 编辑器内容, 外部绑定的v-model

        plugins 要使用的插件, 默认全部

        toolbar1 工具栏第一行

        toolbar2 工具栏第二行

        other_options 其他配置

        uploadImageHandle: 上传图片方法, 参数为file对象

## 三. 事件: 

        input: 内容更新时会触发此事件, 同时触发外部绑定的v-model数据自动更新.

## 四. 方法: 

        暂时未提供供外部调用的方法