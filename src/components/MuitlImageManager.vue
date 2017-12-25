<template>
    <div v-if='active'>
        <div class="mce-container mce-panel mce-floatpanel mce-window mce-in mc-image-panel" hidefocus="1" aria-label="Insert/edit image">
            <div class="mce-reset">
                <div class="mce-window-head">
                    <div class="mce-title">插入图片</div>
                    <div class="mce-dragh"></div>
                    <button type="button" class="mce-close" aria-hidden="true" @click="closePanel">
                        <i class="mce-ico mce-i-remove"></i>
                    </button>
                </div>
                <div class="mce-container-body mce-window-body mce-abs-layout" style="width: 600px; height: 310px;">
                    <ImageUploader v-for="(info, index) in imgList" :key='index' :imgUrl = 'info.imgUrl' :fileName='info.fileName' :status='info.status' :desc='info.desc' />
                    <button class="btn btn-selectImage" @click="selectImage" v-if="imgList.length === 0">选择图片</button>
                </div>
                <div class="mce-container mce-panel mce-foot" hidefocus="1" tabindex="-1" style="border-width: 1px 0px 0px; left: 0px; top: 0px; width: 600px; height: 50px;">
                    <div class="mce-container-body mce-abs-layout" style="width: 600px; height: 50px;">
                        <div class="mce-abs-end"></div>
                        <div class="mce-widget mce-btn mce-primary mce-abs-layout-item mce-first mce-btn-has-text" tabindex="-1" style="left: 470px; top: 10px; width: 50px; height: 28px;">
                            <button type="button" tabindex="-1" style="height: 100%; width: 100%;" @click="submitClick">
                                <span class="mce-txt">确定</span>
                            </button>
                        </div>
                        <div class="mce-widget mce-btn mce-abs-layout-item mce-last mce-btn-has-text" tabindex="-1" style="left: 530px; top: 10px; width: 56px; height: 28px;">
                            <button type="button" tabindex="-1" style="height: 100%; width: 100%;" @click="closePanel">
                                <span class="mce-txt">取消</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <input type="file" style="display: none;" multiple ref='imgUploader' accept="image/png,image/gif,image/jpeg" @change="upload(this)">
        <div id="mce-modal-block" class="mce-reset mce-fade mce-in" style="z-index: 99998;"></div>
    </div>
</template>
<script>
import ImageUploader from './ImageUploader'
export default {
    name: 'multiImgUploader',
    components: { ImageUploader },
    data() {
        return {
            active: false,
            callback: null,
            constrainProportion: true,
            imgList: []
        }
    },
    props: {
        uploadImageHandle: { type: Function, default: null } // 上传方法必须由外部传入
    },
    computed: {
        proportClass() {
            if (this.constrainProportion) {
                return 'mce-checkbox mce-abs-layout-item mce-last mce-checked'
            } else {
                return 'mce-checkbox mce-abs-layout-item mce-last'
            }
        }
    },
    watch: {},
    methods: {
        selectImage() {
            this.$refs.imgUploader.click()
        },
        getImage(callback) {
            this.callback = callback
            this.active = true
        },
        async upload() {
            if (this.$refs.imgUploader.files.length > 0) {
                const files = this.$refs.imgUploader.files
                for (let i = 0; i < files.length; i++) {
                    const file = files[i]
                    const imgInfo = {
                        file,
                        imgUrl: null,
                        fileName: file.name,
                        status: 0,
                        desc: ''
                    }
                    if (!this.uploadImageHandle) {
                        imgInfo.status = 3
                    }
                    this.imgList.push(imgInfo)
                }
                if (!this.uploadImageHandle) {
                    console.log('没有配置上传方法')
                    return
                }
                const imgIndex = 0
                for (let j = 0; j < this.imgList.length; j++) {
                    if (!this.active) return
                    const imgInfo = this.imgList[j]
                    imgInfo.status = 1
                    console.log('start upload')
                    imgInfo.imgUrl = await new Promise(resolve => {
                        this.uploadImageHandle(imgInfo.file)
                            .then(url => {
                                resolve(url)
                            })
                            .catch(e => {
                                resolve(null)
                            })
                    })
                    console.log('end upload')
                    imgInfo.status = imgInfo.imgUrl ? 2 : 3
                }
            }
        },
        closePanel() {
            this.imgList = []
            this.callback = null
            this.active = false
            this.imgUrl = ''
            this.imgAlt = ''
            this.imgWidth = null
            this.imgHeight = null
            this.constrainProportion = true
            this.proportion = 0
        },
        submitClick() {
            for (let i = 0; i < this.imgList.length; i++) {
                if (this.imgList[i].url) {
                    arr.push[
                        {
                            url: this.imgList[i].url,
                            alt: this.imgList[i].fileName
                        }
                    ]
                }
            }
            if (this.callback && typeof this.callback === 'function') {
                this.callback(arr)
            }
            this.$emit('selectImage', arr)
            this.closePanel()
        }
    }
}
</script>
<style scoped>
.mc-image-panel {
    border-width: 1px;
    z-index: 99999;
    margin-left: -300px;
    left: 50%;
    top: 100px;
    width: 600px;
    height: 400px;
}
input[disabled] {
    background: #eee;
}
.mce-container-body {
    padding: 10px;
    overflow-x: hidden;
    overflow-y: auto;
    box-sizing: border-box;
}
.btn-selectImage {
    border: 1px solid #51a351;
    background-color: #5bb75b;
    color: #fff;
    font-size: 16px;
    line-height: 16px;
    padding: 10px;
    cursor: pointer;
}
</style>
