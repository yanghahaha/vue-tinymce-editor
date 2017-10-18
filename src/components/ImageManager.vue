<template>
    <div v-if='active'>
        <div class="mce-container mce-panel mce-floatpanel mce-window mce-in mc-image-panel" hidefocus="1" aria-label="Insert/edit image">
            <div class="mce-reset">
                <div class="mce-window-head">
                    <div class="mce-title">添加/编辑 图片</div>
                    <div class="mce-dragh"></div>
                    <button type="button" class="mce-close" aria-hidden="true" @click="closePanel">
                        <i class="mce-ico mce-i-remove"></i>
                    </button>
                </div>
                <div class="mce-container-body mce-window-body mce-abs-layout" style="width: 425px; height: 140px;">
                    <div class="mce-abs-end"></div>
                    <div class="mce-container mce-form mce-abs-layout-item mce-first mce-last" style="left: 0px; top: 0px; width: 425px; height: 140px;">
                        <div class="mce-container-body mce-abs-layout" style="width: 425px; height: 140px;">
                            <div class="mce-abs-end"></div>
                            <div class="mce-container mce-abs-layout-item mce-first mce-formitem" hidefocus="1" tabindex="-1" style="left: 15px; top: 15px; width: 395px; height: 30px;">
                                <div class="mce-container-body mce-abs-layout" style="width: 395px; height: 30px;">
                                    <div class="mce-abs-end"></div>
                                    <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mceu_44" style="line-height: 16px; left: 0px; top: 7px; width: 142px; height: 16px;">图片地址</label>
                                    <div class="mce-combobox mce-abs-layout-item mce-last mce-has-open" style="left: 142px; top: 0px; width: 253px; height: 30px;">
                                        <input v-if="uploadImageHandle" class="mce-textbox" v-model='imgUrl' hidefocus="1" spellcheck="false" placeholder="" aria-labelledby="mceu_44-l" style="width: 210px;">
                                        <input v-else class="mce-textbox" v-model='imgUrl' hidefocus="1" spellcheck="false" placeholder="" aria-labelledby="mceu_44-l" style="width: 243px;">
                                        <div v-if="uploadImageHandle" class="mce-btn" tabindex="-1" style="margin-left: -4px;">
                                            <button ype="button" hidefocus="1" tabindex="-1" title="选择图片" @click="selectImage">
                                                <i class="mce-ico mce-i-browse"></i>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="mce-container mce-abs-layout-item mce-formitem" hidefocus="1" tabindex="-1" style="left: 15px; top: 55px; width: 395px; height: 30px;">
                                <div class="mce-container-body mce-abs-layout" style="width: 395px; height: 30px;">
                                    <div class="mce-abs-end"></div>
                                    <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mceu_45" style="line-height: 16px; left: 0px; top: 7px; width: 142px; height: 16px;">图片描述(alt)</label><input hidefocus="1" v-model='imgAlt' class="mce-textbox mce-abs-layout-item mce-last" aria-labelledby="mceu_45-l" style="left: 142px; top: 0px; width: 243px; height: 28px;"></div>
                            </div>
                            <div class="mce-container mce-abs-layout-item mce-last mce-formitem" hidefocus="1" tabindex="-1" style="left: 15px; top: 95px; width: 395px; height: 30px;">
                                <div class="mce-container-body mce-abs-layout" style="width: 395px; height: 30px;">
                                    <div class="mce-abs-end"></div>
                                    <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mceu_46" style="line-height: 16px; left: 0px; top: 7px; width: 142px; height: 16px;">图片尺寸</label>
                                    <div class="mce-container mce-abs-layout-item mce-last" aria-labelledby="mceu_46-l" style="left: 142px; top: 0px; width: 253px; height: 30px;">
                                        <div class="mce-container-body mce-abs-layout" style="width: 253px; height: 30px;">
                                            <div class="mce-abs-end"></div><input hidefocus="1" maxlength="5" size="3" class="mce-textbox mce-abs-layout-item mce-first" v-model='imgWidth' aria-label="Width" style="left: 0px; top: 0px; width: 50px; height: 28px;">
                                            <span class="mce-widget mce-label mce-abs-layout-item" style="line-height: 16px; left: 66px; top: 7px; width: 7px; height: 16px;">x</span><input v-model='imgHeight' hidefocus="1" maxlength="5" size="3" class="mce-textbox mce-abs-layout-item" aria-label="Height" style="left: 80px; top: 0px; width: 50px; height: 28px;">
                                            <div :class="proportClass" @click='changeProportSelect' unselectable="on" aria-labelledby="mceu_50-al" tabindex="-1" aria-checked="true" style="left: 155px; top: 6px; width: 157px; height: 18px;">
                                                <i class="mce-ico mce-i-checkbox"></i>
                                                <span class="mce-label">约束比例</span>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="mce-container mce-panel mce-foot" hidefocus="1" tabindex="-1" style="border-width: 1px 0px 0px; left: 0px; top: 0px; width: 425px; height: 50px;">
                    <div class="mce-container-body mce-abs-layout" style="width: 425px; height: 50px;">
                        <div class="mce-abs-end"></div>
                        <div class="mce-widget mce-btn mce-primary mce-abs-layout-item mce-first mce-btn-has-text" tabindex="-1" style="left: 302px; top: 10px; width: 50px; height: 28px;">
                            <button type="button" tabindex="-1" style="height: 100%; width: 100%;" @click="submitClick">
                                <span class="mce-txt">确定</span>
                            </button>
                        </div>
                        <div class="mce-widget mce-btn mce-abs-layout-item mce-last mce-btn-has-text" tabindex="-1" style="left: 357px; top: 10px; width: 56px; height: 28px;">
                            <button type="button" tabindex="-1" style="height: 100%; width: 100%;" @click="closePanel">
                                <span class="mce-txt">取消</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <input type="file" style="display: none;" ref='imgUploader' accept="image/png,image/gif,image/jpeg" @change="upload(this)">
        <div id="mce-modal-block" class="mce-reset mce-fade mce-in" style="z-index: 99998;"></div>
    </div>
</template>
<script>
export default {
    name: 'ImageManager',
    data() {
        return {
            active: false,
            callback: null,
            imgUrl: '',
            imgAlt: '',
            imgWidth: null,
            imgHeight: null,
            constrainProportion: true,
            proportion: 0
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
    watch: {
        imgWidth(val) {
            val = parseInt(val)
            if (isNaN(val)) {
                val = 0
                this.imgWidth = null
                return
            } else {
                this.imgWidth = val
                if (this.proportion > 0 && this.constrainProportion) this.imgHeight = parseInt(val * this.proportion)
            }
        },
        imgHeight(val) {
            val = parseInt(val)
            if (isNaN(val)) {
                val = 0
                this.imgHeight = null
                return
            } else {
                this.imgHeight = val
                if (this.proportion > 0 && this.constrainProportion) this.imgWidth = parseInt(val / this.proportion)
            }
        }
    },
    methods: {
        getImage(callback) {
            this.callback = callback
            this.active = true
        },
        selectImage() {
            this.$refs.imgUploader.click()
        },
        upload() {
            if (this.$refs.imgUploader.files.length > 0) {
                const file = this.$refs.imgUploader.files[0]
                const orignImageName = file.name.replace(/\.jpg|\.png|\.git|\.jpeg/g, '')
                if (this.uploadImageHandle) {
                    this.uploadImageHandle(file).then(url => {
                        this.imgUrl = url
                        this.imgAlt = orignImageName
                        getFileSize(url).then((size) => {
                            this.imgWidth = size.width
                            this.imgHeight = size.height
                            this.proportion = size.width / size.height
                        }).catch((e) => {

                        })
                    }).catch((e) => {
                    })
                }
            }
        },
        changeProportSelect() {
            this.constrainProportion = !this.constrainProportion
            if (this.constrainProportion) this.imgHeight = parseInt(this.imgWidth * this.proportion)
        },
        closePanel() {
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
            const image = {
                url: this.imgUrl,
                alt: this.imgAlt,
                width: this.imgWidth,
                height: this.imgHeight,
            }
            if (this.callback && typeof this.callback === 'function') {
                this.callback( image )
            }
            this.$emit('selectImage', image)
            this.closePanel()
        }
    }
}
function getFileSize(filePath) {
    return new Promise(function(resolve, reject) {
        var image = new Image()
        image.src = filePath
        image.onload = function() {
            resolve({ width: image.width, height: image.height })
        }
    })
}
</script>
<style scoped>
.mc-image-panel {
    border-width: 1px;
    z-index: 99999;
    margin-left: -213px;
    left: 50%;
    top: 100px;
    width: 426px;
    height: 230px;
}
</style>