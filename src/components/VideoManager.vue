<template>
    <div v-if='active'>
        <div class="mc-video-panel mce-container mce-panel mce-floatpanel mce-window mce-in" hidefocus="1" role="dialog" aria-labelledby="mceu_231" aria-describedby="mceu_231-none" aria-label="Insert/edit media">
            <div class="mce-reset" role="application">
                <div class="mce-window-head">
                    <div class="mce-title">添加/编辑 视频</div>
                    <div class="mce-dragh"></div>
                    <button type="button" class="mce-close" aria-hidden="true" @click='hidePanel'>
                        <i class="mce-ico mce-i-remove"></i>
                    </button>
                </div>
                <div class="mce-container-body mce-window-body mce-abs-layout" style="width: 512px; height: 166px;">
                    <div class="mce-abs-end"></div>
                    <div class="mce-container mce-panel mce-abs-layout-item mce-first mce-last" hidefocus="1" tabindex="-1" style="left: 0px; top: 0px; width: 512px; height: 166px;">
                        <div class="mce-tabs" role="tablist">
                            <div :class="this.tabIndex === 1? 'mce-tab mce-active' : 'mce-tab'" @click="setTabIndex(1)" unselectable="on" role="tab" aria-controls="mceu_233" aria-selected="true" tabindex="-1">常规</div>
                            <div :class="this.tabIndex === 2? 'mce-tab mce-active' : 'mce-tab'" @click="setTabIndex(2)" class="mce-tab" style="margin-left:-4px" unselectable="on" role="tab" aria-controls="mceu_240" aria-selected="false" tabindex="-1">代码嵌入</div>
                        </div>
                        <div class="mce-container-body mce-abs-layout" style="width: 512px; height: 136px;">
                            <div class="mce-abs-end"></div>
                            <div v-if='tabIndex === 1' class="mce-container mce-form mce-abs-layout-item mce-first" role="tabpanel" aria-labelledby="mceu_232-t0" style="left: 0px; top: 0px; width: 512px; height: 136px;">
                                <div class="mce-container-body mce-abs-layout" style="width: 512px; height: 136px;">
                                    <div class="mce-abs-end"></div>
                                    <div class="mce-container mce-abs-layout-item mce-first mce-formitem" hidefocus="1" tabindex="-1" style="left: 20px; top: 20px; width: 472px; height: 30px;">
                                        <div class="mce-container-body mce-abs-layout" style="width: 472px; height: 30px;">
                                            <div class="mce-abs-end"></div>
                                            <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mceu_234" style="line-height: 16px; left: 0px; top: 7px; width: 103px; height: 16px;">视频地址</label>
                                            <div class="mce-combobox mce-abs-layout-item mce-last mce-has-open" style="left: 103px; top: 0px; width: 369px; height: 30px;"><input class="mce-textbox" v-model="videoUrl" value="" hidefocus="1" spellcheck="false" size="40" placeholder="" aria-labelledby="mceu_234-l" style="width: 326px;">
                                                <i class="mce-status mce-ico" style="display: none"></i>
                                                <div class="mce-btn mce-open" tabindex="-1" role="button" style='margin-left:-3px;'>
                                                    <button type="button" hidefocus="1" tabindex="-1" @click="browseFile">
                                                        <i class="mce-ico mce-i-browse"></i>
                                                    </button>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="mce-container mce-abs-layout-item mce-last mce-formitem" hidefocus="1" tabindex="-1" style="left: 20px; top: 60px; width: 472px; height: 30px;">
                                        <div class="mce-container-body mce-abs-layout" style="width: 472px; height: 30px;">
                                            <div class="mce-abs-end"></div>
                                            <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mceu_235" style="line-height: 16px; left: 0px; top: 7px; width: 103px; height: 16px;">视频尺寸</label>
                                            <div class="mce-container mce-abs-layout-item mce-last" aria-labelledby="mceu_235-l" style="left: 103px; top: 0px; width: 369px; height: 30px;">
                                                <div class="mce-container-body mce-abs-layout" style="width: 369px; height: 30px;">
                                                    <div class="mce-abs-end"></div><input v-model="videoWidth" hidefocus="1" maxlength="5" size="5" class="mce-textbox mce-abs-layout-item mce-first" aria-label="Width" style="left: 0px; top: 0px; width: 41px; height: 28px;">
                                                    <span class="mce-widget mce-label mce-abs-layout-item" style="line-height: 16px; left: 56px; top: 7px; width: 7px; height: 16px;">x</span><input v-model='videoHeight' hidefocus="1" maxlength="5" size="5" class="mce-textbox mce-abs-layout-item" aria-label="Height" style="left: 68px; top: 0px; width: 41px; height: 28px;">
                                                    <!-- <div class="mce-checkbox mce-abs-layout-item mce-last mce-checked" unselectable="on" aria-labelledby="mceu_239-al" tabindex="-1" role="checkbox" aria-checked="true" style="left: 124px; top: 6px; width: 157px; height: 18px;">
                                                        <i class="mce-ico mce-i-checkbox"></i>
                                                        <span class="mce-label">约束比例</span>
                                                    </div> -->
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div v-if='tabIndex === 2' class="mce-container mce-abs-layout-item" role="tabpanel" aria-labelledby="mceu_232-t1" style="left: 0px; top: 0px; width: 512px; height: 136px;">
                                <div class="mce-container-body mce-abs-layout" style="width: 512px; height: 136px;">
                                    <div class="mce-abs-end"></div>
                                    <label class="mce-widget mce-label mce-abs-layout-item mce-first" for="mcemediasource" style="line-height: 16px; left: 10px; top: 10px; width: 492px; height: 16px;">把播放器代码粘贴在下面:</label>
                                    <textarea v-model='embedString' id="mcemediasource" hidefocus="1" rows="5" class="mce-textbox mce-multiline mce-abs-layout-item mce-last" style="left: 10px; top: 36px; width: 482px; height: 80px;"></textarea>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="mce-container mce-panel mce-foot" hidefocus="1" tabindex="-1" role="group" style="border-width: 1px 0px 0px; left: 0px; top: 0px; width: 512px; height: 50px;">
                    <div class="mce-container-body mce-abs-layout" style="width: 512px; height: 50px;">
                        <div class="mce-abs-end"></div>
                        <div class="mce-widget mce-btn mce-primary mce-abs-layout-item mce-first mce-btn-has-text" tabindex="-1" role="button" style="left: 385px; top: 10px; width: 50px; height: 28px;">
                            <button role="presentation" type="button" tabindex="-1" style="height: 100%; width: 100%;" @click='submitClick'>
                                <span class="mce-txt">确定</span>
                            </button>
                        </div>
                        <div class="mce-widget mce-btn mce-abs-layout-item mce-last mce-btn-has-text" tabindex="-1" role="button" style="left: 440px; top: 10px; width: 60px; height: 28px;">
                            <button role="presentation" type="button" tabindex="-1" @click='hidePanel' style="height: 100%; width: 100%;">
                                <span class="mce-txt">取消</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div id="mce-modal-block" class="mce-reset mce-fade mce-in" style="z-index: 99998;"></div>
    </div>
</template>
<script>
export default {
    name: 'VideoManager',
    data() {
        return {
            active: false,
            tabIndex: 1,
            callback: null,
            videoUrl: '',
            videoWidth: null,
            videoHeight: null,
            embedString: ''
        }
    },
    props: {
    },
    computed: {
    },
    watch: {
        videoUrl(newUrl, oldUrl) {
            if (newUrl === '') {
                this.videoHeight = this.videoWidth = null
            }
            if (oldUrl === '') {
                this.videoWidth = 320
                this.videoHeight = 160
            }
        }
    },
    methods: {
        getVideo(callback) {
            this.active = true
            this.callback = callback
        },
        hidePanel() {
            this.active = false
            this.tabIndex = 1
            this.callback = null
            this.videoUrl = ''
            this.videoWidth = null
            this.videoHeight = null
            this.embedString = ''
        },
        setTabIndex(val) {
            this.tabIndex = val
        },
        browseFile() {
            alert('暂不支持视频文件上传')
        },
        submitClick() {
            let tagString = ''
            if (this.tabIndex == 1) {
                tagString = `<video style="background: #eee;" controls="controls" controls="controls" width="${this.videoWidth}" height="${this.videoHeight}"> <source src="${this.videoUrl}" type="video/mp4" /></video>`
            } else {
                tagString = this.embedString
            }
            if (this.callback && typeof this.callback === 'function') {
                this.callback(tagString)
            }
            this.hidePanel()
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
.mc-video-panel {
    border-width: 1px;
    z-index: 99999;
    margin-left: -256px;
    left: 50%;
    top: 100px;
    width: 512px;
    height: 256px;
}
</style>
<style>

</style>
