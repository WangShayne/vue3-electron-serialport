<template>
    <n-card size="large">
        <n-form :label-width="80" :model="formValue" :rules="rules" :size="size" ref="formRef"></n-form>
        <n-form inline>
            <n-grid :cols="24" :x-gap="24">
                <n-form-item-gi :span="6" label="方法" path="phone">
                    <n-radio-group name="radiogroup">
                        <n-space>
                            <n-radio key="gonglv" value="功率法">功率法</n-radio>
                            <n-radio key="wucha" value="误差法">误差法</n-radio>
                        </n-space>
                    </n-radio-group>
                </n-form-item-gi>
                <n-form-item-gi :span="5" label="编程密码" path="phone">
                    <n-input placeholder="000000002"></n-input>
                </n-form-item-gi>
                <n-form-item-gi :span="5" label="编程密码" path="phone">
                    <n-input placeholder="000000002"></n-input>
                </n-form-item-gi>
                <n-form-item-gi :span="6" label="误差" path="phone">
                    <n-checkbox-group>
                        <n-space item-style="display: flex;">
                            <n-checkbox value="Beijing" label="误差" />
                            <n-checkbox value="Shanghai" label="RTC" />
                        </n-space>
                    </n-checkbox-group>
                </n-form-item-gi>

                <n-form-item-gi :span="4">
                    <n-button @click="handleValidateClick" attr-type="button">自动校表</n-button>
                </n-form-item-gi>
                <n-form-item-gi :span="4">
                    <n-button @click="handleValidateClick" attr-type="button">停止</n-button>
                </n-form-item-gi>
            </n-grid>
        </n-form>
    </n-card>
    <n-card size="large" title="校正过程监控">
        <n-log
            :rows="5"
            :log="`
            0.98f3d3e62ed8d
            0.e01fdc6b903ac
            0.aeacc9ac81f4d
            0.ae0561c16ca6e
            0.cf54299ffb031
            0.590e1e70becd6
            0.00486d2c2423f
            0.2618dafebd0a
            0.e5321786c566e
            0.676eed4888833
            0.e7f4aacb50226
            0.644affc9a1f07
            0.067f530b828f9
            0.688a7e157fc79
            0.7b65ae2b2168a
            0.3b328e51baaf
            0.999f65b390cbd
            0.d78940ea49f4f
            0.5e2b6c207adc8
            0.c391529466753
            0.cfd3d8a4d351f
            0.0696a011a7367
            0.f4ec745bbb583
            0.ce280678a5566
            0.2467743d46369
            0.4d9e434fa2884
            0.864173df70e08
            0.b71b98f9899b4
            0.57a114cdf7ced
            0.697c2b138daed
            0.8e72bf615f3f9
            0.6ff88d11ecd58
            0.2c2c8d9b4d9e6
            0.a19e0fc4945d5
            0.30108f7b4497a
            0.7723a1cd5ad37
            0.bef157f384b58
            0.7389f574b6cf1
            0.d90c8e28e8941
            0.8ce0984ccb6cc`"
        />
    </n-card>
    <n-card size="large" title="校正结果">
        <n-data-table :columns="columns" :data="data" :pagination="pagination" />
    </n-card>
</template>

<script>
import { h, defineComponent, ref } from 'vue'
// import { BuildOutline } from '@vicons/ionicons5'
import { NTag, NButton, useMessage } from 'naive-ui'
// import mMeter from '../../data/mMeter.json'

// console.log(mMeter)
const createColumns = ({ sendMail }) => {
    return [
        {
            title: 'Name',
            key: 'name',
            align: 'center'
        },
        {
            title: 'Age',
            key: 'age'
        },
        {
            title: 'Address',
            key: 'address'
        },
        {
            title: 'Tags',
            key: 'tags',
            render(row) {
                const tags = row.tags.map((tagKey) => {
                    return h(
                        NTag,
                        {
                            style: {
                                marginRight: '6px'
                            },
                            type: 'info'
                        },
                        {
                            default: () => tagKey
                        }
                    )
                })
                return tags
            }
        },
        {
            title: 'Action',
            key: 'actions',
            render(row) {
                return h(
                    NButton,
                    {
                        size: 'small',
                        onClick: () => sendMail(row)
                    },
                    { default: () => 'Send Email' }
                )
            }
        }
    ]
}
const createData = () => [
    {
        key: 0,
        name: 'John Brown',
        age: 32,
        address: 'New York No. 1 Lake Park',
        tags: ['nice', 'developer']
    },
    {
        key: 1,
        name: 'Jim Green',
        age: 42,
        address: 'London No. 1 Lake Park',
        tags: ['wow']
    },
    {
        key: 2,
        name: 'Joe Black',
        age: 32,
        address: 'Sidney No. 1 Lake Park',
        tags: ['cool', 'teacher']
    }
]
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
        // BuildOutline
    },
    setup() {
        const formRef = ref(null)
        const message = useMessage()
        return {
            data: createData(),
            columns: createColumns({
                sendMail(rowData) {
                    message.info('send mail to ' + rowData.name)
                }
            }),
            pagination: {
                pageSize: 10
            },
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
