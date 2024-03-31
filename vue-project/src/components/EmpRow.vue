<template>
    <tr>
        <th scope="row">{{ idx + 1 }}</th>
        <td>{{ empData.empNo }}</td>
        <td>{{ empData.name }}</td>
        <td class="email">{{ empData.email }}</td>
        <td>{{ empData.mobile }}</td>
        <td>
            <button type="button" class="btn btn-primary" @click="modifyEmp(idx)">編輯</button>
            <button type="button" class="btn btn-danger" @click="delEmp">刪除</button>
        </td>
    </tr>

</template>

<script setup>
const props = defineProps(['idx', 'empData'])
const emit = defineEmits(['empDelEvent', 'empModEvent'])

function modifyEmp(idx) {
    //呼叫parent的empModifyEvent
    emit('empModEvent', idx)
}

function delEmp(idx) {
    if (confirm('確定要刪除嗎?')) {
        //呼叫parent的delEmpEvent
        emit('empDelEvent', idx)
    }
}


</script>

<style scoped>
.email {
    color: blue;
    text-transform: uppercase;
}

</style>