<template>
    <div ref="message" class="messageBox">
        <div class="title">{{title}}</div>
        <div class="content">{{content}}</div>
    </div>
</template>

<script>
export default {
    name: 'message',
    props: {
        title: String,
        content: String,
    },
    data() {
        return {
            timer: null, // 定时器
        }
    },
    methods: {
        dismissMessageBox() {
            // 定时器将弹框上移动10px然后渐渐消失
                this.timer = setTimeout(()=>{
                      // 上移动
                    this.$refs.message.style.transform="translateY(-200px)";
                    this.$refs.message.style.opacity="0"; 
                    clearTimeout(this.timer); 
                }, 1000);
        },
    },
    mounted() {
        this.dismissMessageBox();
    },
    created() {
    },
}
</script>
<style lang="scss" scoped>
    .messageBox {
        width: 200px;
        padding: 15px 10px;
        position: absolute;
        bottom: 0;
        right: 0;
        z-index: 99;
        transition: 5s;
        background: linear-gradient(to right, #84fab0, #8fd3f4);
        color: #fff;
        border-radius: 15px 0px 0px 15px;
        opacity: .85;
        // transform: translate(0, -200px);
        .title {
            text-align: left;
            font-weight: bold;
            font-size: 20px;
            line-height: 20px;

        }
        .content {
            font-size: 18px;
            text-align: left;
            line-height: 25px;
        }
    }
</style>