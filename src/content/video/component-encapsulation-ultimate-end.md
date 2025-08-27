# vue 组件的二次封装 - 究极版

<div class="tip custom-block">

视频讲解

* [抖音](https://www.douyin.com/video/7543238747513441575)
* [哔哩哔哩](https://www.bilibili.com/video/BV1bDe1z1Eyr)

</div>

### 核心代码

```vue
<template>
    <div>
        <h1>我自定义的内容</h1>
        <component :is="h(ElInput,{...$attrs,ref:changeRef},$slots)"/>
    </div>
</template>

<script lang="ts" setup>
    import {type ComponentInstance, type ComponentInternalInstance, getCurrentInstance, h} from "vue";
    import {ElInput} from "element-plus";

    defineOptions({
        name: 'MyInput'
    })

    defineProps<{ aaa: string }>()


    /**
     * 组件二次封装：
     * 1. 属性
     * 2. 事件
     * 3. 插槽
     * 4. 方法
     * 5. 类型
     */

    const vm = getCurrentInstance() as ComponentInternalInstance

    function changeRef(instance: Record<string, any> | null) {
        vm.exposed = vm.exposeProxy = instance || {}
    }

    /**
     * 不幸的消息：
     * 这个方式只支持 vs code 的提示，因为它是基于 vue language tools。
     * 不支持 webstorm 的提示，因为它没有全部集成 vue language tools。
     */
    defineExpose({} as ComponentInstance<typeof ElInput>)
</script>
```

### 使用 Demo

```vue
<script lang="ts" setup>
    import {ref, useTemplateRef} from "vue";
    import MyInput from "@/components/MyInput.vue";

    const modelValue = ref('hello world')
    const inputRef = useTemplateRef('inputRef')

    setTimeout(() => {
        inputRef.value?.clear()
    }, 1000)

</script>

<template>
    <div>
        <MyInput
            ref="inputRef"
            v-model="modelValue"
            :aaa="1"
            :show-passworld="true"
            placeholder="dddd"
        >
            <template #append>asdasd</template>
            <template #prefix>前缀</template>
        </MyInput>
    </div>
</template>
```
