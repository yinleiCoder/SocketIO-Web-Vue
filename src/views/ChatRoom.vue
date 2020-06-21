<template>
    <div class="chatRoom">
        <Header class="my-header" title="I Miss You~"></Header>
        <div class="online-member">当前聊天室在线人数：<span>{{member}}</span>人</div>
        <div class="main">
            <!--<div v-for="(item, index) in arr" :key="index">{{item}}</div>-->
                <div v-for="(item, index) in chat" :key="index">
                <div class="left msg" v-if="item.id === 1">
                    <img :src="item.img"/>
                    <div class="nr">
                        <div class="neir">
                            {{item.neir}}
                        </div>
                        <div class="nt">{{item.name}} {{item.time}}</div>
                    </div>
                </div>
                <div class="right msg" v-if="item.id === 2">
                    <img :src="item.img"/>
                    <div class="nr">
                        <div class="neir">
                            {{item.neir}}
                        </div>
                        <div class="nt">{{item.name}} {{item.time}}</div>
                    </div>
                </div>
                <div class="center msg" v-if="item.id === 3">
                    <div class="inner">{{item.name}}</div>
                </div>
            </div>
        </div>

        <div class="foot">
            <input type="text" class="cont" v-model="chatm"/>
            <!-- <button @click="send()">提交</button> -->
            <img src="../assets/send.png" @click="send()"/>
        </div>
    </div>
</template>
<script>
import Header from '@/components/Header.vue';
export default {
    name: 'chatRoom',
    components: {
        Header,
    },
   
    data() {
        return {
             // 聊天内容
            chatm: '',// 消息内容
            name: '',//用户名称
            img: '',// 用户头像
            chat1: [],// 聊天内容
            member: 0, // 成员个数
            chat: [],

            // arr: ['Hi girl! good night!', 'Hi boy! you are handsome today!', 'thank you, could you please invite me to your bed?'],
            // cont: '',
            // id: '',
            avatarImgs: [require('../assets/avatar.jpg'), require('../assets/avatar2.jpg'),require('../assets/avatar3.jpg'),require('../assets/avatar4.jpg'),require('../assets/avatar5.jpg'),require('../assets/avatar6.jpg'),
            require('../assets/avatar7.jpg'),require('../assets/avatar8.jpg'),require('../assets/avatar9.jpg'),require('../assets/avatar10.jpg')],
        }
    },
    sockets: {
        connect: function () {
            this.id = this.$socket.id;
            console.log(`socket connected: ${this.id}`);
        },
        customEmit: function (data) {
            console.log('this method was fired by the socket server. eg: io.emit("customEmit", data)'+data)
        }
    },
    methods: {
        // 接收广播消息
        receiveMsg() {
            // this.$socket.on('gbmsg', data => {
            //     alert(data);
            //     this.arr.push(data);
            // })
            this.sockets.listener.subscribe('gbmsg', data => {
                this.arr.push(data);
            })
        },
        convertTime(date){
            // let tempTime = date.toLocaleTimeString();
            return date.toLocaleTimeString();
        },
        // 加入聊天室
        join(name){
            this.$socket.emit('join', name);
        },
        // 欢迎加入
        welcome() {
            this.sockets.listener.subscribe('welcome', ([name, member]) => {
                let data = {
                    name: `欢迎 ${name} 加入群聊`,
                    id: 3,
                };
                this.chat.push(data);
                this.member = member;
            });
        },
        send() {
            // if(this.cont.length > 0){
            //     this.arr.push(this.cont);
            //     let content = this.cont;
            //     // 发送
            //     this.$socket.emit('message',content);
            // }
            if(this.chatm.length>0){
                let msg =  {
                    // name: this.name,
                    neir: this.chatm,
                    img: this.img,
                    time: new Date().toLocaleTimeString(),
                    id: 2
                };
                let data =  {
                    name: this.name,
                    neir: this.chatm,
                    img: this.img,
                    time: new Date().toLocaleTimeString(),
                    id: 1
                };
                this.chat.push(msg);
                this.$socket.emit('message', data);
                this.chatm = '';
            }
        },
        // 获取群消息
        getMsg() {
            this.sockets.listener.subscribe('sendMsg', data => {
                console.log(data);
                this.chat.push(data);
            });
        },
        // 接收用户退出的广播
        quit(){
            this.sockets.listener.subscribe('quit', ([name, member]) => {
                let data =  {
                    name: `${name} 已退出群聊`, 
                    id: 3
                };
                this.member= member;
                this.chat.push(data);
            });
        }
    },
    created() {
        this.$socket.emit('connect','connect background of socket!!!');

        // 获取路由传递过来的参数
        // console.log(this.$route.query.name,);
        // console.log(this.$route.query.img)
        this.img = this.avatarImgs[this.$route.query.img]; // 拿到当前用户头像
        this.name = this.$route.query.name; //用户名
    },
    mounted() {
        // 加入聊天室
        this.join(this.$route.query.name);
        this.welcome();
        this.quit();
        // this.receiveMsg();
        this.getMsg();
        // console.log(this.sockets.listener);
    },
}
</script>

<style lang="scss" scoped>
    .chatRoom {
        display: flex;
        flex-direction: column;
        .my-header{
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            // margin-bottom: 42px;
        }
        .online-member{
            position: fixed;
            top: 45px;
            left: 0;
            right: 0;
            & > span {
                color: orange;
                font-weight: bold;

            }
        }
        .main {
            flex: 1;
            overflow-y: auto;
            margin-top: 65px;
            margin-bottom: 71px;
            .msg {
                width: 100%;
                // height: 85px;
                // border: 1px solid red;
                display: flex;
                margin-bottom: 20px;
                .nr {
                    display: flex;
                    flex-direction: column;
                    justify-content: flex-end;
                    align-items: flex-start;
                    .neir {
                        width: 224px;
                        padding: 13px 16px 11px 16px;
                        box-sizing: border-box;
                        font-size: 14px;
                        font-family:PingFangSC-Regular,PingFang SC;
                        font-weight:400;
                        color:rgba(25,29,35,1);
                        line-height:20px;
                        contenteditable: true; // 这个可以让div模拟textarea换行
                    }
                    .nt {
                        height: 17px;
                        margin-top: 4px;
                        font-size: 12px;
                        font-family:PingFangSC-Regular,PingFang SC;
                        font-weight: 400;
                        color: rgba(25, 29, 35, .5);
                        line-height: 17px;
                    }
                }
                & > img {
                    width: 32px;
                    height: 32px;
                    border-radius: 9px;
                    margin: 10px;
                }
                &.right{
                    flex-direction: row-reverse;
                    align-items: center;
                    justify-content: flex-start;
                    & .nr .neir {
                        background:rgba(244,244,244,1);
                        border-radius:12px 12px 12px 0px;
                        text-align: left;
                    }
                }
                &.left{
                    flex-direction: row;
                    justify-content: flex-start;
                    align-items: center;
                        & .nr .neir {
                    background:rgba(255,234,222,1);
                    border-radius:12px 12px 0px 12px;
                    text-align: left;
                    }
                }
                &.center{
                    margin-top:20px;
                    margin-bottom: 20px;
                    flex-direction: row;
                    justify-content: center;
                    align-items: center;
                    color: #e74c3c;
                    font-weight: bold;
                }
            }
        }
        .foot {
            width: 100vw;
            display: flex;
            flex-direction: row;
            align-items: center;
            position: fixed;
            bottom: 0;
            margin: auto 22px 25px 24px;
            .cont {
                // height: 44px;
                // flex: 8;
                width: fill-available;
                max-width: 283px;
                contenteditable: true;
                border-radius: 12px;
                background:rgba(242,242,242,1);
                border: 1px solid #f5f5f5;
                outline: none;
                padding: 12px;
                font-size: 14px;
                font-family:PingFangSC-Regular,PingFang SC;
                font-weight:400;
                color:rgba(25,29,35,1);
                line-height:20px;
                margin-right: 13px;
            }
            & > img {
                // flex: 2;
                width: 20px;
                height: 20px;
            }
            // .cont {
            //     flex: 8;
            // }
            // & > button {
            //     flex: 2;
            // }
            
        }
    }
</style>