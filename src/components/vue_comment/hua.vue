<template>
    <div class="drag" ref="drag" id="drag" :style="drag">
        <div class="drag_bg" id="drag_bg" ref="drag_bg" :style="drag_bg_style"></div>
        <div class="drag_text" ref="drag_text" :style="drag_text_style">{{confirmWords}}</div>
        <div @mousedown="mousedownFn($event)" id="handler" ref="handler" :class="handler_class"
             :style="handler_style"></div>
    </div>
</template>

<script>
    export default {
        name: 'hua',
        components: {},
        props: {},
        data() {
            return {
                beginClientX: 0, /*距离屏幕左端距离*/
                mouseMoveStata: false, /*触发拖动状态  判断*/
                maxwidth: 218, /*拖动最大宽度，依据滑块宽度算出来的  滑块宽度-42PX  */  //258-60
                confirmWords: '拖动滑块验证', /*滑块文字*/
                confirmSuccess: false, /*验证成功判断*/
                handler_class: {
                    handler: true,
                    handler_bg: true,
                    handler_ok_bg: false,
                },
                handler_style: {
                    left: 0,
                },
                drag_bg_style: {
                    width: 0,
                },
                drag_text_style: {
                    width: 260
                },
                drag: {
                    color: "#333",
                },
                events: {
                    handleEvent: function (event) {
                        switch (event.type) {
                            case 'mousemove':
                                this.myMouseMove(event);
                                break;
                            case 'mouseup':
                                this.myMouseUp(event);
                                break;
                        }
                    },
                    myMouseMove: (e) => {
                        if (this.mouseMoveStata) {
                            var width = e.clientX - this.beginClientX;
                            if (width > 0 && width <= this.maxwidth) {
                                this.$refs.handler.style.left = width + "px";
                                this.$refs.drag_bg.style.width = width + "px";

                            } else if (width > this.maxwidth) {
                                this.successFunction();
                            }
                        }
                    },
                    myMouseUp: (e) => {
                        this.mouseMoveStata = false;
                        let width = e.clientX - this.beginClientX;
                        if (width < this.maxwidth) {
                            this.$refs.handler.style.left = "0px";
                            this.$refs.drag_bg.style.width = "0px";
                        }
                    },
                }
            }
        },
        methods: {
            mousedownFn: function (e) {
                this.mouseMoveStata = true;
                this.beginClientX = e.clientX;
            },
            successFunction() {
                //成功回调
                // console.log("成功回调--");
                this.handler_class.handler_bg = false;
                this.handler_class.handler_ok_bg = true;
                this.confirmWords = '验证通过';
                this.drag.color = "#fff";
                this.$refs.handler.style.left = this.maxwidth + "px";
                this.$refs.drag_bg.style.width = this.maxwidth + "px";
                
				let btn2 = document;
                btn2.removeEventListener('mousemove', this.events, false);
                btn2.removeEventListener('mouseup', this.events, false);

                //把文字宽度改为230px，让它居中
                this.$refs.drag_text.style.width = 230 + "px";
                this.confirmSuccess = true;
                this.$emit('verify_pass');
            },                //验证成功函数
            //重置验证码状态
            reset_verify() {
                console.log("开始重置验证码状态");
                this.handler_class.handler_bg = true;
                this.handler_class.handler_ok_bg = false;
                this.confirmWords = '拖动滑块验证'
                this.drag.color = "#333";
                this.$refs.handler.style.left = "0px";
                this.$refs.drag_bg.style.width = "0px";
                this.mouseMoveStata = false;
                
                let btn2 = document;
                btn2.addEventListener('mousemove', this.events, false);
                btn2.addEventListener('mouseup', this.events, false);

                //把文字宽度改为260px，让它居中
                this.$refs.drag_text.style.width = "260px";
            },

            //绑定的滑块拖动事件。
            mouseMove_event(ev) {
                let e = ev || event;
                if (this.mouseMoveStata) {
                    console.log("滑块拖动");
                    let width = ev.clientX - this.beginClientX;
                    console.log("width--" + width);
                    if (width > 0 && width <= this.maxwidth) {
                        this.$refs.handler.style.left = width + "px";
                        this.$refs.drag_bg.style.width = width + "px";
                    } else if (width > this.maxwidth) {
                        this.successFunction();
                    }
                }
            },
            //绑定的鼠标松开事件。
            mouseUp_event(ev) {
                let e = ev || event;
                console.log("鼠标松开");
                this.mouseMoveStata = false;
                let width = ev.clientX - this.beginClientX;
                // let width = document.getElementById("drag_bg").offsetWidth - this.beginClientX;
                console.log("width--" + width);
                if (width < this.maxwidth) {
                    this.$refs.handler.style.left = "0px";
                    this.$refs.drag_bg.style.width = "0px";
                }
            },
        },
        mounted() {
            var drag = document;
            drag.addEventListener('mousemove', this.events, false);
            drag.addEventListener('mouseup', this.events, false);
        }
    }
</script>

<style scoped>
    .drag {
        display: inline-block;
        position: relative;
        background-color: #e8e8e8;
        width: 260px;
        height: 34px;
        margin-left: 30px;
        /*margin-top: 100px;*/
        line-height: 34px;
        text-align: center;
        /*禁止选中*/
        user-select: none;
    }

    .handler {
        position: absolute;
        top: 0px;
        left: 0px;
        width: 43px;
        height: 34px;
        border: 1px solid #ccc;
        cursor: move;
    }

    .handler_bg {
        background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTEyNTVEMURGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTEyNTVEMUNGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo2MTc5NzNmZS02OTQxLTQyOTYtYTIwNi02NDI2YTNkOWU5YmUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+YiRG4AAAALFJREFUeNpi/P//PwMlgImBQkA9A+bOnfsIiBOxKcInh+yCaCDuByoswaIOpxwjciACFegBqZ1AvBSIS5OTk/8TkmNEjwWgQiUgtQuIjwAxUF3yX3xyGIEIFLwHpKyAWB+I1xGSwxULIGf9A7mQkBwTlhBXAFLHgPgqEAcTkmNCU6AL9d8WII4HOvk3ITkWJAXWUMlOoGQHmsE45ViQ2KuBuASoYC4Wf+OUYxz6mQkgwAAN9mIrUReCXgAAAABJRU5ErkJggg==") no-repeat center;
    }

    .handler_ok_bg {
        background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NDlBRDI3NjVGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NDlBRDI3NjRGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDphNWEzMWNhMC1hYmViLTQxNWEtYTEwZS04Y2U5NzRlN2Q4YTEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+k+sHwwAAASZJREFUeNpi/P//PwMyKD8uZw+kUoDYEYgloMIvgHg/EM/ptHx0EFk9I8wAoEZ+IDUPiIMY8IN1QJwENOgj3ACo5gNAbMBAHLgAxA4gQ5igAnNJ0MwAVTsX7IKyY7L2UNuJAf+AmAmJ78AEDTBiwGYg5gbifCSxFCZoaBMCy4A4GOjnH0D6DpK4IxNSVIHAfSDOAeLraJrjgJp/AwPbHMhejiQnwYRmUzNQ4VQgDQqXK0ia/0I17wJiPmQNTNBEAgMlQIWiQA2vgWw7QppBekGxsAjIiEUSBNnsBDWEAY9mEFgMMgBk00E0iZtA7AHEctDQ58MRuA6wlLgGFMoMpIG1QFeGwAIxGZo8GUhIysmwQGSAZgwHaEZhICIzOaBkJkqyM0CAAQDGx279Jf50AAAAAABJRU5ErkJggg==") no-repeat center;
    }

    .drag_bg {
        background-color: #7ac23c;
        height: 34px;
        width: 0px;
    }

    .drag_text {
        position: absolute;
        top: 0px;
        width: 260px;
        -moz-user-select: none;
        -webkit-user-select: none;
        user-select: none;
        -o-user-select: none;
        -ms-user-select: none;
    }
</style>
