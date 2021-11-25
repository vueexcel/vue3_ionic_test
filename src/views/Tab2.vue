<template>
  <ion-page>
    <ion-header class="ion-no-border">
      <ion-toolbar>
        <ion-title><h3>Chats</h3></ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-list>
        <ion-item lines="none" @click="() => router.push(`/chatPage/${friend.id}`)" v-for="friend in friendsListData" :key="friend.id">
          <ion-avatar slot="start">
            <img :src="friend.profileImage">
          </ion-avatar>
          <ion-label class="ion-text-nowrap">
            <h2>{{friend.name}}</h2>
            <p>{{friend.lastMessage}}</p>
            <div class="info-div">
              <p>13:02</p>
              <div class="noOfMessage">2</div>
            </div>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonAvatar, IonLabel } from '@ionic/vue';
import { useRouter } from 'vue-router';
import { ref } from '@vue/reactivity';
import { mount } from '@vue/test-utils';
import { onUpdated } from 'vue';

export default  {
  name: 'Tab2',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonAvatar, IonLabel },
  setup () {
    const router = useRouter()
    const friendsListData = ref([] as any)
    const actualChat = ref([] as any)
    if (!localStorage.getItem("chatMessagesData")) {
      const chatData = [
         {
          senderId: 1,
          receiverId:2,
          sentBy:'sender',
          attachedImage: "https://i.pravatar.cc/150?img=8",
          textMsg: "Hello there, how are you ?",
          timeStamp: "10:32",
        },
        {
          senderId: 4,
          receiverId:2,
          sentBy:'sender',
          attachedImage: "",
          textMsg: "I m good and you ? how is your work going on",
          timeStamp: "10:33",
        },
        {
          senderId: 3,
          receiverId:2,
          sentBy:'sender',
          attachedImage: "",
          textMsg: "I m fine thanks",
          timeStamp: "10:33",
        },
        {
          senderId: 5,
          receiverId:2,
          sentBy:'sender',
          attachedImage: "https://i.pravatar.cc/150?img=15",
          textMsg: "what is going on",
          timeStamp: "10:34",
        }
      ];
      localStorage.setItem("chatMessagesData", JSON.stringify(chatData));
    }

    if(!localStorage.getItem('friendListData')){
      const friendList = [
      {id:1, profileImage:'https://i.pravatar.cc/150?img=31',name:'Minsk', lastMessage:'Hey there, I am using this app'},
      {id:3, profileImage:'https://i.pravatar.cc/150?img=21',name:'Moscow', lastMessage:'Hey there, I am using this app'},
      {id:4, profileImage:'https://i.pravatar.cc/150?img=1',name:'Kiev', lastMessage:'Hey there, I am using this app'},
      {id:5, profileImage:'https://i.pravatar.cc/150?img=11',name:'Peter', lastMessage:'Hey there, I am using this app'}
    ]
      localStorage.setItem('friendListData', JSON.stringify(friendList))
    }

    const getFriendsData = () => {
      actualChat.value = JSON.parse(
        localStorage.getItem("chatMessagesData") || "[]"
      );

      friendsListData.value = JSON.parse(localStorage.getItem('friendListData') || '[]')

      friendsListData.value.forEach((element: { id: string|string[]; lastMessage: any }) => {
        const array: any[] = []
        actualChat.value.forEach((el: { senderId: string|string[] }) => {
        if(el.senderId == element.id){
          array.push(el)
        }
      });
      element.lastMessage = array[array.length-1].textMsg
      });
    }
  
    onUpdated(() => {
      getFriendsData()
    })

    getFriendsData()
    return { mount, router, friendsListData }
  }
}
</script>

<style scoped>
ion-item{
  margin-left: 5%;
  margin-right: 5%;
  border-bottom:1px solid #ccc;
}
ion-avatar{
  margin-left: -10px;
}
ion-label p{
  opacity: 0.8;
  font-size: 0.8rem;
}
.info-div{
  position: absolute;
  right:0;
  top:0.8rem;
}
.info-div p{
  opacity: 0.5;
  font-size: 0.7rem;
}
.info-div .noOfMessage{
  background-color: blue;
  color:white;
  font-size: 0.7rem;
  padding-top: 0.1rem;
  padding-left: 0.25rem;
  height: 1rem;
  width: 1rem;
  border-radius: 50%;
  margin-left: 0.5rem;
}
</style>