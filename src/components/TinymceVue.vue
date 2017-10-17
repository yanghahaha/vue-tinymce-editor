<template>
    <div :id='randomID'>
        <textarea :id="editorID" v-model="content"></textarea>
        <ImageManager ref='imageManager' :uploadImage='uploadImage' />
        <VideoManager ref='videoManager' />
    </div>
</template>

<script>
// 导入tinymce
import tinymce from 'tinymce/tinymce.min'

// theme, 必须导入, 否则报错
import 'tinymce/themes/modern/theme';

// 想要使用的插件需要在这里应用
import 'tinymce/plugins/advlist';
import 'tinymce/plugins/wordcount';
import 'tinymce/plugins/autolink';
import 'tinymce/plugins/autosave';
import 'tinymce/plugins/charmap';
import 'tinymce/plugins/codesample';
import 'tinymce/plugins/contextmenu';
import 'tinymce/plugins/emoticons';
import 'tinymce/plugins/codesample';
import 'tinymce/plugins/fullscreen';
import 'tinymce/plugins/hr';
import 'tinymce/plugins/imagetools';
import 'tinymce/plugins/insertdatetime';
import 'tinymce/plugins/link';
import 'tinymce/plugins/media';
import 'tinymce/plugins/noneditable';
import 'tinymce/plugins/paste';
import 'tinymce/plugins/print';
import 'tinymce/plugins/searchreplace';
import 'tinymce/plugins/tabfocus';
import 'tinymce/plugins/template';
import 'tinymce/plugins/textpattern';
import 'tinymce/plugins/visualblocks';
import 'tinymce/plugins/anchor';
import 'tinymce/plugins/autoresize';
import 'tinymce/plugins/bbcode';
import 'tinymce/plugins/code';
import 'tinymce/plugins/colorpicker';
import 'tinymce/plugins/directionality';
import 'tinymce/plugins/fullpage';
import 'tinymce/plugins/help';
import 'tinymce/plugins/image';
import 'tinymce/plugins/importcss';
import 'tinymce/plugins/legacyoutput';
import 'tinymce/plugins/lists';
import 'tinymce/plugins/nonbreaking';
import 'tinymce/plugins/pagebreak';
import 'tinymce/plugins/preview';
import 'tinymce/plugins/save';
import 'tinymce/plugins/spellchecker';
import 'tinymce/plugins/table';
import 'tinymce/plugins/textcolor';
import 'tinymce/plugins/toc';
import 'tinymce/plugins/visualchars';

import 'tinymce/skins/lightgray/skin.min.css'
import 'tinymce/skins/lightgray/content.min.css'
import 'tinymce/skins/lightgray/fonts/tinymce.woff'

// 汉化包
import '../assets/langs/zh_CN'

// 导入图片管理和视频管理组件(自定义vue组件, 使用原tinymce组件的样式,重新定义的功能)
import ImageManager from './ImageManager'
import VideoManager from './VideoManager'

export default {
    name: 'tinymce',
    props: {
        'htmlClass': { default: '', type: String },
        'value': { default: '' },
        'plugins': {
            default: function() {
                return [
                    'advlist autolink lists link image charmap print preview hr anchor pagebreak ' +
                    'searchreplace wordcount visualblocks visualchars code fullscreen ' +
                    'insertdatetime media nonbreaking save table contextmenu directionality ' +
                    'template paste textcolor colorpicker textpattern imagetools toc help emoticons '
                ];
            }, type: Array
        },
        toolbar1: { default: 'fullscreen code preview | formatselect removeformat | bold italic underline strikethrough forecolor backcolor fontselect fontsizeselect | anchor charmap link unlink | alignleft aligncenter alignright alignjustify  | numlist bullist outdent indent  | table insertImage insertVideo', type: String },
        toolbar2: { default: '', type: String },
        other_options: {
            default: function() {
                return {
                    font_formats: 'Verdana=Verdana;微软雅黑=Microsoft YaHei;宋体=SimSun;黑体=SimHei;仿宋=FangSong;楷体=kaiTi;隶书=LiSu;幼圆=YouYuan;',
                };
            }, type: Object
        },
        uploadImage: { type: Function, default: null }
    },
    data() {
        return {
            randomID: `ej_editor_container_${Math.random() * 9999999999999999}`,
            editorID: `ej_editor_${Math.random() * 99999999999999999}`,
            content: '',
            editor: null,
            cTinyMce: null,
            checkerTimeout: null,
            isTyping: false
        };
    },
    components: {
        ImageManager,
        VideoManager
    },
    mounted() {
        this.content = this.value;
        this.init();
    },
    beforeDestroy() {
        if(this.editor) this.editor.destroy();
    },
    watch: {
        value: function(newValue) {
            if (!this.isTyping) {
                if (this.editor !== null) {
                    this.editor.setContent(newValue)
                } else {
                    this.content = newValue
                }
            }
        },
        // content: function(val) {
        //     if (this.editor !== null) {
        //         this.editor.setContent(val)
        //     }
        // }
    },
    methods: {
        init() {
            const editorHeight = document.getElementById(this.randomID).offsetHeight
            console.log(this.editorID, this.randomID)
            const that = this
            let options = {
                selector: `#${this.editorID}`,
                skin: false,
                branding: false,
                toolbar1: this.toolbar1,
                toolbar2: this.toolbar2,
                menubar: false,
                plugins: this.plugins,
                height: editorHeight,
                init_instance_callback: (editor) => {
                    this.editor = editor;
                    editor.on('KeyUp', (e) => {
                        this.submitNewContent();
                    });
                    editor.on('Change', (e) => {
                        if (this.editor.getContent() !== this.value) {
                            this.submitNewContent();
                        }
                    });
                    editor.on('init', (e) => {
                        editor.setContent(this.content);
                        this.$emit('init', this.content);
                    });
                },
                setup(editor) {
                    editor.addButton('insertImage', {
                        tooltip: '插入图片',
                        icon: 'image',
                        onclick: () => {
                            that.$refs.imageManager.getImage(
                                (img) => {
                                    editor.insertContent(`<img src="${img.url}" alt="${img.alt}" width="${img.width}" height="${img.height}" />`)
                                }
                            )
                        }
                    })
                    editor.addButton('insertVideo', {
                        tooltip: '插入视频',
                        icon: 'media',
                        onclick: () => {
                            that.$refs.videoManager.getVideo((tagStr)=>{
                                editor.insertContent(tagStr)
                            })
                        }
                    })
                },
            };
            tinymce.init(this.concatAssciativeArrays(options, this.other_options));
        },
        concatAssciativeArrays(array1, array2) {
            if (array2.length === 0) return array1;
            if (array1.length === 0) return array2;
            let dest = [];
            for (let key in array1) dest[key] = array1[key];
            for (let key in array2) dest[key] = array2[key];
            return dest;
        },
        submitNewContent() {
            this.isTyping = true;
            if (this.checkerTimeout !== null)
                clearTimeout(this.checkerTimeout);
            this.checkerTimeout = setTimeout(() => {
                this.isTyping = false;
            }, 300);

            this.$emit('input', this.editor.getContent());
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.mce-tinymce {
    /* height: 100%; */
}
.mce-floatpanel[aria-label=Preview] {
    width: 90%;
    left: 5% !important;
    /* left: 0 !important; */
}
</style>