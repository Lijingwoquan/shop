<template>
    <div v-if="modelValue">
        <el-avatar :src="modelValue" fit="cover" class="w-[100px] h-[100px] rounded border mr-2"></el-avatar>
    </div>
    <div class="choose-image-btn" @click="open">
        <el-icon :size="25" class="text-gray-500">
            <Plus />
        </el-icon>
    </div>

    <el-dialog title="选择图片" v-model="dialogVisable" width="80%" top="5vh">
        <el-container class="bg-white rounded" style="height: 70vh;">
            <el-header class="image-header">
                <el-button type="primary" size="small" @click="handleOpenCreate">新增图片分类</el-button>
                <el-button type="warning" size="small" @click="handleOpenUpload">上传图片</el-button>
            </el-header>
            <el-container>
                <ImageAside ref="ImageAsideRef" @change="handelAsideChange"></ImageAside>
                <ImageMain :openChoose="true" ref="ImageMainRef" @choose="handleChoose"></ImageMain>
            </el-container>
        </el-container>

        <template #footer>
            <span>
                <el-button type="primary" @click="cancel">取消</el-button>
                <el-button type="primary" @click="submit">确定</el-button>
            </span>
        </template>
    </el-dialog>

</template>

<script setup>
import { ref } from "vue";
import ImageAside from '~/components/imageAside.vue';
import ImageMain from '~/components/imageMain.vue';
const dialogVisable = ref(false)
const ImageAsideRef = ref(null)
const ImageMainRef = ref(null)
const handleOpenCreate = () => ImageAsideRef.value.handleCreate()
const handleOpenUpload = () => ImageMainRef.value.openUploadFile()
const handelAsideChange = (imageClassID) => {
    ImageMainRef.value.loadData(imageClassID)
}

const props = defineProps({
    modelValue: [String, Array]
})
const emit = defineEmits(["update:modelValue"])


const open = () => dialogVisable.value = true
const close = () => dialogVisable.value = false

const submit = () => {
    if (urls.length) {
        emit("update:modelValue", urls[0])
        close()
    }
}

const cancel = () => {
    dialogVisable.value = false
}

let urls = []
const handleChoose = (e) => {
    urls = e.map(o => o.url)
}
</script>

<style scoped>
    .choose-image-btn {
        @apply w-[100px] h-[100px] rounded border flex justify-center items-center cursor-pointer hover:(bg-gray-100);
    }

    .image-header {
        border-bottom: 1px solid #eeeeee;
        @apply flex items-center;
    }
</style>