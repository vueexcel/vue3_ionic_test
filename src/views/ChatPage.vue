<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button color="primary" @click="goBack()"></ion-back-button>
        </ion-buttons>
        <ion-item lines="none">
          <ion-avatar slot="start">
            <img :src="currentFriendData.profileImage" />
          </ion-avatar>
          <ion-label class="ion-text-nowrap">
            <h2>{{ currentFriendData.name }}</h2>
            <p>Last message at 13:02</p>
          </ion-label>
        </ion-item>
      </ion-toolbar>
    </ion-header>
     <ion-content :scroll-events="true" ref="myContent"> 
      <div class="container" v-if="chatMessages">
        <div
          class="mainMessageWrapper"
          v-for="chat in chatMessages"
          :key="chat"
        >
         <div class="senderMessage" v-if="chat.sentBy == 'sender'">
            <div class="senderProfile">
              <img :src="currentFriendData.profileImage" />
            </div>
            <div class="messageContent">
              <div class="attachedImage" v-if="chat.attachedImage">
                <img :src="chat.attachedImage" />
              </div>
              <div class="rowMessage">
                <div class="rowMessage">
                  <div class="textMsg">
                    <p>{{ chat.textMsg }}</p>
                  </div>
                  <div class="messageTime">
                    <p>{{ chat.timeStamp }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

        <div class="recieverMessage" v-else>
            <div class="messageContent">
              <div class="attachedImage" v-if="chat.attachedImage">
                <img :src="chat.attachedImage" />
              </div>
              <div class="rowMessage">
                <div class="messageTime">
                  <p>{{ chat.timeStamp }}</p>
                </div>
                <div class="textMsg">
                  <p>{{ chat.textMsg }}</p>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </ion-content>
    <ion-footer>
      <ion-toolbar>
        <div>
          <ion-list lines="none" class="ion-no-margin">
            <ion-item>
              <ion-chip color="primary">
                <ion-icon :icon="attachOutline" />
              </ion-chip>
              <ion-chip color="primary">
                <ion-icon :icon="micOutline" />
              </ion-chip>
              <ion-input
                type="text"
                placeholder="Type your message..."
                v-model="userMessage"
                @keyup.enter="sendMessage()"
              ></ion-input>
            </ion-item>
          </ion-list>
        </div>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonAvatar,
  IonLabel,
  IonChip,
  IonInput,
} from "@ionic/vue";
import { arrowBack, micOutline, attachOutline } from "ionicons/icons";
import { useRoute, useRouter } from "vue-router";
import { computed, ref } from "@vue/reactivity";
import { watch } from '@vue/runtime-core';
// import { watch } from 'vue';
export default {
  name: "chatPage",
  components: {
    IonHeader,
    IonToolbar,
    IonPage,
    IonAvatar,
    IonLabel,
    IonChip,
    IonInput,
  },
  setup() {
    const userMessage = ref("");
    const router = useRouter()
    const route = useRoute();
    const myContent = ref(null);
    const currentFriend = ref({} as any);
    const chatMessages = ref([] as any);
    const actualChat = ref([] as any)
    

    const getFilteredMessages = () => {
      chatMessages.value = []
      actualChat.value = JSON.parse(
      localStorage.getItem("chatMessagesData") || "[]"
    );
    actualChat.value.forEach((element: { senderId: string|string[] }) => {
      if(element.senderId == route.params.friendId){
        chatMessages.value.push(element)
      }
    });
    }

    getFilteredMessages()
    const currentFriendData = computed(() => {
      const friendList = JSON.parse(
        localStorage.getItem("friendListData") || "[]"
      );
      friendList.forEach((element: { id: string | string[] }) => {
        if (element.id == route.params.friendId) {
          currentFriend.value = element;
        }
      });
      return currentFriend.value;
    });


    watch(route, (value) => {
      if (value.params.friendId) {
        getFilteredMessages()
      }
    })

    const sendMessage = () => {
      const obj = {
        senderId: route.params.friendId,
        receiverId: 2,
        sentBy: 'receiver',
        attachedImage: "",
        textMsg: userMessage.value,
        timeStamp: "16:14",
      }
     
    actualChat.value.push(obj)
      localStorage.setItem(
        "chatMessagesData",
        JSON.stringify(actualChat.value)
      );
      userMessage.value = "";
      getFilteredMessages()
    };

    const goBack = () => {
      chatMessages.value = []
      currentFriend.value = {};
      router.push("/tabs/tab2");
    };
    return {
      arrowBack,
      micOutline,
      attachOutline,
      router,
      chatMessages,
      sendMessage,
      userMessage,
      myContent,
      currentFriendData,
      goBack,
      getFilteredMessages
    };
  },
};
</script>

<style scoped>
ion-content {
  padding-bottom: 50px !important;
}
ion-item {
  padding-left: 0;
}
ion-avatar {
  height: 2.3rem;
  width: 2.3rem;
  margin-left: -15px;
}
ion-label p {
  opacity: 0.8;
  font-size: 0.7rem;
}
ion-chip {
  padding: 5px 1px;
}
ion-chip ion-icon {
  margin-right: 5px;
}

ion-input {
  margin-left: 1rem;
  border-bottom: 1px solid #ccc;
  font-size: 0.8rem;
}

/* message box design */

.senderMessage {
  width: 94%;
  display: flex;
  flex-wrap: wrap;
  height: auto;
  position: relative;
  margin: 3% auto;
}
.senderMessage .senderProfile {
  bottom: 0px;
  left: 0;
  position: absolute;
}

.senderMessage .senderProfile img {
  height: 35px;
  width: 35px;
  border-radius: 15%;
}

.senderMessage .messageContent {
  background: #f4f6f9;
  border-radius: 15px;
  min-width: 50%;
  max-width: 90%;
  padding: 0.3rem 0.8rem;
  margin-left: 15%;
}
.senderMessage .messageContent .rowMessage {
  font-size: 0.8rem;
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.textMsg {
  width: 80%;
}
.textMsg p {
  width: auto;
}
.senderMessage .messageContent .messageTime {
  width: 18%;
  margin-left: 2%;
  position: relative;
}
.messageTime p {
  font-size: 0.6rem;
  opacity: 0.4;
  bottom: 5px;
  right: 0;
  position: absolute;
}

/*reciever message box design*/
.recieverMessage {
  width: 94%;
  float: right;
  height: auto;
  position: relative;
  margin-right: 4%;
  margin-bottom: 3%;
}

.recieverMessage .messageContent {
  background: #e7effc;
  border-radius: 15px;
  min-width: 50%;
  max-width: 80%;
  padding: 0.3rem 0.8rem;
  float: right;
  color: #0c5eda;
}
.recieverMessage .messageContent .rowMessage {
  font-size: 0.8rem;
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.attachedImage {
  width: 100%;
  text-align: center;
}
.attachedImage img {
  margin: 1% auto;
  border-radius: 0.6rem;
}
.recieverMessage .messageContent .messageTime {
  width: 18%;
  margin-right: 2%;
  position: relative;
}
.recieverMessage .messageContent .messageTime p {
  font-size: 0.6rem;
  opacity: 0.5;
  bottom: 4px;
  left: 0;
  position: absolute;
}
</style>