<template>
<div class="imageUploader">
    <div class="holder">
        <div class="content">
            <img src="" alt="">
        </div>
        <div class="head">
            <div class="title" v-html="fileName"></div>
        </div>
        <div class="body">
            <div :class="statusClass" v-html="statusText"></div>
        </div>
        <div class="foot">
            <div class="desc" v-html="desc"></div>
        </div>
    </div>
</div>

</template>
<script>
export default {
    name: 'ImageUploader',
    props: {
        imgUrl: { type: String, default: '' },
        fileName: { type: String, default: '测试' },
        status: { type: Number, default: 0 },
        desc: { type: String, default: 'test' }
    },
    computed: {
        statusText() {
            return ['等待上传', '图片上传中...', '上传成功', '上传失败'][this.status]
        },
        statusClass() {
            return 'status ' + ['waiting', 'progressing', 'success', 'failed'][this.status]
        }
    },
    methods: {
        closeBtnClicked() {
            console.log('close button just been clicked')
        }
    }
}
</script>
<style scoped>
.imageUploader {
    width: 178px;
    height: 138px;
    padding: 5px;
    float: left;
}
.holder {
    width: 100%;
    height: 100%;
    position: relative;
    box-sizing: border-box;
    border: 1px solid #ccc;
    background-repeat: repeat;
    background-image: url('../assets/alpha_bg.jpg');
    text-align: center;
}
.head {
    position: absolute;
    width: 100%;
    height: 20px;
    top: 0;
    left: 0;
}
.body {
    height: 100%;
    width: 100%;
}
.foot {
    position: absolute;
    width: 100%;
    height: 20px;
    left: 0;
    bottom: 0;
}
.head,
.foot {
    background-color: #333;
    opacity: 0.5;
}
.title,
.status,
.desc {
    width: 100%;
    text-align: center;
}
.title,
.desc {
    color: #fff;
}
.status {
    line-height: 100px;
    font-weight: 800;
    color: #666;
}
.status.progressing {
    color: #006;
}
.status.success {
    color: #060;
}
.status.failed {
    color: #600;
}
.mce-close {
    position: absolute;
    right: 5px;
    top: 2px;
    cursor: pointer;
}
.mce-close i {
    color: #fff;
}
</style>

