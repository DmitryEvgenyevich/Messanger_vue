<script setup lang="ts">
import { NAvatar, NButton, NFlex, NGrid, NGridItem, NIcon, NText } from 'naive-ui';
import { PhoneIcon, InformationCircleIcon } from '@heroicons/vue/24/outline';
import { useStore } from '../stores/store';
import { ChatType, IGroupChat, IPrivateChat } from '../models';


const store = useStore();

function onInfoButtonClick()
{
    store.showUserInfo = !store.showUserInfo;
}

function getAvatarLink(): string {
    let link = "https://upload.wikimedia.org/wikipedia/commons/9/99/Sample_User_Icon.png";

    if(store.selectedChat!.type_id === ChatType.Group && (store.selectedChat as IGroupChat).avatar_url != null)
    {
        link = (store.selectedChat as IGroupChat).avatar_url!;
    }
    else if(store.selectedChat!.type_id === ChatType.Private && (store.selectedChat as IPrivateChat).user.avatar_url != null)
    {
        link = (store.selectedChat as IPrivateChat).user.avatar_url!;
    }

    return link
}

</script>

<template>
    <NFlex class="w-full p-5px b-b-1px b-b-solid b-b-#EFEFF5 items-center">
        <NFlex>
            <NAvatar 
                round
                :size="43"
                :src="getAvatarLink()"
            />
        </NFlex>
        <NFlex>
            <NGrid :cols="1">
                <NGridItem>
                    <NText strong>
                        {{
                            store.selectedChat?.type_id === ChatType.Group ? 
                            (store.selectedChat as IGroupChat).chat_title : 
                            store.selectedChat?.type_id === ChatType.Private ? 
                            (store.selectedChat as IPrivateChat).user.username : 
                            ''
                        }}
                    </NText>
                </NGridItem>
                <NGridItem>
                    <NText>lastSeenOnline</NText>
                </NGridItem>
            </NGrid>
        </NFlex>
        <NFlex class="m-l-auto">
            <NGrid :cols="1">
                <NGridItem class="justify-right flex">
                    <NButton :bordered="false" circle size="medium" ghost color="#007AFF">
                        <template #icon>
                            <NIcon :size="23"><PhoneIcon/></NIcon>
                        </template>
                    </NButton>
                    <NButton @click="onInfoButtonClick" :bordered="false" circle size="medium" ghost color="#898989">
                        <template #icon>
                            <NIcon :size="25"><InformationCircleIcon/></NIcon>
                        </template>
                    </NButton>
                </NGridItem>
            </NGrid>
        </NFlex>
    </NFlex>
</template>

<style>
</style>