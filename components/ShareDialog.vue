<script setup>
import ChatGPTLogo from '@/assets/chatgpt.svg'

const props = defineProps({
  appBar: {
    type: Boolean,
    required: true
  },
  conversation: {
    type: Object,
    required: true
  }
})

const { isMobile } = useDevice();
const user = useUser()
const dialog = ref(false)
const hideUserName = ref(true)

const getDate = () => {
  const now = new Date()
  return now.toLocaleDateString('en-US')
}

</script>

<template>
  <v-dialog
    v-if="appBar"
    v-model="dialog"
    persistent
    max-width="768"
  >
    <template v-slot:activator="{ props }">
      <v-btn
        v-bind="props"
        :title="$t('share')"
        icon="share"
        variant="text"
        class="mr-4"
      ></v-btn>
    </template>
    <v-card>
      <v-toolbar>
        <v-toolbar-title>{{ $t('shareConversation') }}</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn
          icon="close"
          variant="text"
          @click="dialog = false"
        ></v-btn>
      </v-toolbar>
      <v-card-text v-show="!conversation.shared">{{ $t("shareTips1") }}</v-card-text>
      <v-card-text v-show="conversation.shared">{{ $t("shareTips2") }}</v-card-text>
      <div class="share-window">
        <div
          class="d-flex flex-column justify-space-between share-panel"
          style="width: 100%;"
        >
          <div class="d-flex flex-column flex-grow-1 ml-4 mr-4">
            <div
              v-for="(message, index) in conversation.messages" :key="index"
              class="d-flex align-start"
              :class="message.is_bot ? 'justify-start' : 'justify-end'"
            >
              <div v-if="message.is_bot && !isMobile" class="avatar-bot">
                <img :src="ChatGPTLogo" alt="ChatGPT">
              </div>
              <MsgContent
                :message="message"
                :message-index="index"
                :use-prompt="usePrompt"
                :retry-message="retryMessage"
                :delete-message="deleteMessage"
                :style="`max-width: ${isMobile ? '95%' : '75%'};`"
                :editable="false"
              />
              <div v-if="!message.is_bot && !isMobile" class="avatar-user">
                <div class="avatar-text"><v-icon icon="fa:fa-solid fa-user" size="28"></v-icon></div>
              </div>
            </div>
          </div>
        </div>
        <div class="share-option">
          <div class="share-topic">{{ conversation.topic }}</div>
          <div class="share-info">
            <span v-if="!hideUserName" class="mr-2">{{ user.username }} · </span>
            <span>{{ getDate() }}</span>
            <v-spacer></v-spacer>
            <div class="d-flex align-center">
              <v-switch
                v-model="hideUserName"
                inline
                color="primary"
                hide-details=""
              ></v-switch>
              <span class="ml-3">{{ $t('showUserName') }}</span>
            </div>
          </div>
        </div>
      </div>
      <v-card-action class="d-flex justify-end mr-4 mb-3">
        <v-btn
          prepend-icon="fa:fa-solid fa-link"
          variant="outlined"
        >{{ $t('share') }}</v-btn>
      </v-card-action>
      
    </v-card>
  </v-dialog>
</template>

<style scoped>
.share-window {
  border-radius: 10px;
  border: 1px solid rgba(161, 158, 158, 0.5);
  margin: 10px 25px 20px 25px;
  box-shadow: 0 0 5px rgba(170, 189, 181, 0.305);
}
.share-panel {
  max-height: 400px;
  overflow-y: auto;
  padding: 0 0 15px 0;
}
.share-option {
  font-size: 1.1rem;
  border-top: 1px solid rgba(161, 158, 158, 0.5);
  padding: 15px 30px 0 30px;
}
.share-topic {
  font-weight: bold;
}
.share-info {
  color: gray;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.avatar-bot {
  margin: 22px 10px 0 10px;
  padding: 8px 8px 2px 8px;
  border-radius: 10px;
  background-color: rgb(25, 195, 125);
  transform: scale(0.7);
}
.avatar-user {
  margin: 26px 10px 0 10px;
  border-radius: 10px;
  background-color: rgb(100, 130, 45);
  transform: scale(0.8);
}
.avatar-text {
  width: 51px;
  height: 51px;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5rem;
}
</style>