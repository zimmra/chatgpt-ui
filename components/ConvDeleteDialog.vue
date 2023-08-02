<template>
    <v-dialog v-model="localIsShow" persistent max-width="768">
        <v-card>
            <v-card-title>
                <span class="text-h5">{{ $t('confirmDeleteConversation') }}</span>
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text>
                {{ $t('confirmDeleteConversationDetails') + pageTitle }}
            </v-card-text>
            <v-divider></v-divider>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" @click="onConfirm()"> {{ $t('Confirm deletion') }} </v-btn>
                <v-btn color="primary" @click="onCancel()"> {{ $t('Cancel deletion') }} </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script setup>
const props = defineProps({
    isShow: Boolean,
    items: Object,
    cIdx: Number,
})
const { isShow, items, cIdx } = toRefs(props)

const localIsShow = ref(isShow.value)

const { $i18n } = useNuxtApp()

const pageTitle = computed(() => {
    const title = items.value[cIdx.value]?.topic

    if(title){
        return title
    }

    return $i18n.t('defaultConversationTitle')
})

watch(isShow, (newValue) => {
    localIsShow.value = newValue;
});

const emit = defineEmits([
    'on:confirm', 'update:isShow'
]);

function onConfirm() {
    emit('on:confirm', items.value, cIdx.value)
    onCancel()
}

function onCancel() {
    localIsShow.value = false
    emit('update:isShow', localIsShow.value)
}
</script>
