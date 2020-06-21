<template>
    <div class="home">
        <message-box v-if="isVisible"  title="Error tip" content="Your userName must not be empty!"></message-box>
        <Header title="Baby, Welcome to join us!!!"></Header>
        <div class="userAvatar">
            <img class="head-img" :src="avatarUrl"/>
            <img @click="select"  src="../assets/select.png" />
        </div>
        <input type="text" class="userName" placeholder="enter your user's name" v-model="userName"/>
        <div class="join" @click="submit">Join Us</div>

        <div class="modify" ref="model" @click="dismiss">
            <div v-for="(item, index) in avatarImgs" :key="index" class="user-img" @click="selected(index)">
                <img :src="item"/>
            </div>
        </div>
    </div>
</template>
<script>
import Header from '@/components/Header.vue';
import MessageBox from '@/components/MessageBox.vue';
export default {
    name: 'Home',
    components: {
        Header,
        MessageBox
    },
    data() {
        return {
            avatarUrl: require('../assets/avatar.jpg'),
            userName: '',
            avatarImgs: [require('../assets/avatar.jpg'), require('../assets/avatar2.jpg'),require('../assets/avatar3.jpg'),require('../assets/avatar4.jpg'),require('../assets/avatar5.jpg'),require('../assets/avatar6.jpg'),
            require('../assets/avatar7.jpg'),require('../assets/avatar8.jpg'),require('../assets/avatar9.jpg'),require('../assets/avatar10.jpg')],
            isVisible: false, // 是否需要显示提示框
        }
    },
    methods: {
        dismiss() {
            this.$refs.model.style.opacity='0';
        },
        select() {
            this.$refs.model.style.opacity='1';
        },
        selected(index){ // 选择头像
            // console.log(index)
            this.avatarUrl = this.avatarImgs[index];
        },
        submit(){
            // this.$router.push({path: ' 路由 ', query: {key: value}})
            if(this.userName !== '') { // 跳转chatRoom
                let tempAvatarIndex = this.avatarImgs.indexOf(this.avatarUrl);
                // console.log(tempAvatarIndex);
                this.$router.push({path: 'chatroom',query: {name: this.userName, img: tempAvatarIndex}});
            } else {
                this.isVisible = !this.isVisible;
            }
        }
    },
}
</script>
<style lang="scss" scoped>
.home {
    display: flex;
    position: relative;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    .userAvatar {
        margin-top: 90px;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 58px;
        & > img:first-child  {
            height: 72px;
            width: 72px;
            border-radius: 50%;
            margin-right: 8px;
        }
        & > img:last-child  {
            height: 11px;
            width: 11px;
        }
    }
    .userName{
        width: 240px;
        height: 48px;
        background:rgba(242,242,242,1);
        border-radius: 15px;
        padding: 12px;
        outline: none;
        border: none;
        text-align: center;
        font-weight:600;
        font-size: 15px;
        font-family:PingFangSC-Semibold,PingFang SC;
        color:rgba(25,29,35,1);
        line-height:15px;
        margin-bottom: 98px;
    }
    .join {
        width: 100px;
        height: 100px ;
        font-size: 20px;
        font-family:PingFangSC-Semibold,PingFang SC;
        font-weight:600;
        color:rgba(255,255,255,1);
        line-height: 20px;
        background:rgba(255,131,60,1);
        box-shadow:0px 16px 34px -4px rgba(255,131,60,0.5);
        // padding: 40px 0;
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
    }
    .modify {
        width: 100vw;
        // height: 0px;
        position: absolute;
        bottom: 0;
        left: 0;
        opacity: 0;
        transition: 1.5s;
        z-index: 20;
        padding-left: 10px;
        padding-top: 36px;
        padding-bottom: 36px;
        box-shadow: 0px 16px 34px -4px rgba(255,131,60,0.5);
        background: linear-gradient(45deg, #92fe9d, #00c9ff);
        border-radius: 22px 22px 0px 0px;
        display: grid;
        grid-template-columns: repeat(5,1fr);
        grid-template-rows: repeat(2, 1fr);
        grid-gap: 20px 20px;
        .user-img {
            width:40px;
            height:40px;
            img {
                width:100%;
                height:100%;
                border-radius:9px;
            }
        }
    }
}
</style>