<template>
        <n-card size="large">
            <n-thing content-indented>
                <template #avatar>
                    <n-avatar>
                        <n-icon color="#0e7a0d">
                            <BuildOutline />
                        </n-icon>
                    </n-avatar>
                </template>
                <template #header><n-gradient-text type="success" :size="20" style="font-weight:600"> 端口选择 </n-gradient-text></template>
                <template #description>
                    <n-form
                        :label-width="80"
                        :model="formValue"
                        :rules="rules"
                        :size="size"
                        ref="formRef"
                    ></n-form>
                    <n-form inline size="small">
                        <n-grid :cols="24" :x-gap="24">
                            <n-form-item-gi :span="24" label="表位设置" path="formValue.name">
                                <n-cascader
                                    v-model:value="formValue.mMeter"
                                    multiple
                                    clearable
                                    placeholder="没啥用的值"
                                    :expand-trigger="'click'"
                                    :options="options"
                                    @update:value="handleUpdateValue"
                                />
                            </n-form-item-gi>
                            <n-form-item-gi :span="10" label="波特率" path="phone">
                                <n-select v-model:value="boardSelect" :options="boardOptions" />
                            </n-form-item-gi>
                            <n-form-item-gi :span="10" label="校验位" path="phone">
                                <n-select v-model:value="modeSelect" :options="boardOptions" />
                            </n-form-item-gi>

                            <n-form-item-gi>
                                <n-button @click="handleValidateClick" attr-type="button">打开串口</n-button>
                            </n-form-item-gi>
                        </n-grid>
                    </n-form>
                </template>
            </n-thing>
        </n-card>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { BuildOutline } from '@vicons/ionicons5'
import { useMessage } from 'naive-ui'
// import mMeter from '../../data/mMeter.json'

// console.log(mMeter)

function getOptions(depth = 3, iterator = 1, prefix = '') {
    const length = 12
    const options = []
    for (let i = 1; i <= length; ++i) {
        if (iterator === 1) {
            options.push({
                value: `v-${i}`,
                label: `l-${i}`,
                disabled: i % 5 === 0,
                children: getOptions(depth, iterator + 1, '' + String(i))
            })
        } else if (iterator === depth) {
            options.push({
                value: `v-${prefix}-${i}`,
                label: `l-${prefix}-${i}`,
                disabled: i % 5 === 0
            })
        } else {
            options.push({
                value: `v-${prefix}-${i}`,
                label: `l-${prefix}-${i}`,
                disabled: i % 5 === 0,
                children: getOptions(depth, iterator + 1, `${prefix}-${i}`)
            })
        }
    }
    return options
}
export default defineComponent({
    components: {
        BuildOutline
    },
    setup() {
        const formRef = ref(null)
        const message = useMessage()
        return {
            formRef,
            size: ref('small'),
            formValue: ref({
                mMeter: "medium",
                phone: ''
            }),
            rules: {},
            options: getOptions(),
            handleUpdateValue(...args) {
                console.log(...args)
            },
            boardSelect: [],
            boardOptions: [
                {
                    label: "1200",
                    value: '1200'
                },
                {
                    label: "2400",
                    value: '2400'
                },
                {
                    label: "5400",
                    value: '5400'
                },
                {
                    label: "9600",
                    value: '9600'
                },
            ],
            modeSelect: [],
            modeOptions: [
                {
                    label: "None 无",
                    value: 'None'
                },
                {
                    label: "Even 偶",
                    value: 'Even'
                },
                {
                    label: "Odd 奇",
                    value: 'Odd'
                },
            ],
            // rules: {
            //     user: {
            //         name: {
            //             required: true,
            //             message: '请输入姓名',
            //             trigger: 'blur'
            //         },
            //         age: {
            //             required: true,
            //             message: '请输入年龄',
            //             trigger: ['input', 'blur']
            //         }
            //     },
            //     phone: {
            //         required: true,
            //         message: '请输入电话号码',
            //         trigger: ['input']
            //     }
            // },
            handleValidateClick() {
                formRef.value.validate((errors) => {
                    if (!errors) {
                        message.success('Valid')
                    } else {
                        console.log(errors)
                        message.error('Invalid')
                    }
                })
            }
        }
    },
})
</script>
