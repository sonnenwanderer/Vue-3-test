<template>
    <div>
        <div class="col-sm">
            <h1>已建立員工清單</h1>
        </div>
    </div>
    <div class="container-fluid">
        <table class="table">
            <thead class="table-dark">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">員工編號</th>
                    <th scope="col">姓名</th>
                    <th scope="col">Email</th>
                    <th scope="col">Mobile</th>
                    <th scope="col">動作</th>
                </tr>
            </thead>
            <tbody>
                <empRow v-for="(empData, idx) in empList" :key="idx" :idx="idx" :empData="empData"
                @empDelEvent="delEmp" @empModEvent="modifyEmp">
                </empRow>
            </tbody>

        </table>
    </div>

    <!-- 利用Modal選單修改員工資料 -->
    <div class="modal" id="myModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">員工資訊</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <table class="table">
                        <tbody>
                            <tr class="row">
                                <th class="col">員工編號</th>
                                <td><input type="text" v-model="modalEmpData.empNo"></td>
                            </tr>
                            <tr class="row">
                                <th class="col">姓名</th>
                                <td><input type="text" v-model="modalEmpData.name"></td>
                            </tr>
                            <tr class="row">
                                <th class="col">Email</th>
                                <td><input type="text" v-model="modalEmpData.email"></td>
                            </tr>
                            <tr class="row">
                                <th class="col">Mobile</th>
                                <td><input type="text" v-model="modalEmpData.mobile"></td>
                            </tr>
                        </tbody>
                    </table> 
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="saveEmpData">Save changes</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script setup>
import { computed, normalizeClass, ref } from 'vue'
import EmpRow from './EmpRow.vue'
import * as bootstrap from 'bootstrap'

const empList = ref([
    { empNo: '813234', name: 'John', email: 'john@cht.com.tw', mobile: '0912345678' },
    { empNo: '603235', name: 'Mary', email: 'mary@cht.com.tw', mobile: '0923456789' },
    { empNo: '046236', name: 'Tom', email: 'tom@cht.com.tw', mobile: '0934567890' },
    { empNo: '037937', name: 'Jerry', email: 'jerry@cht.com.tw', mobile: '0945678901' },
    { empNo: '400239', name: 'Mike', email: 'mike@cht.com.tw', mobile: '0956789012' },
])

const modalEmpIdx = ref(null)
const modalEmpData = ref({ empNo: '', name: '', email: '', mobile: '' })

computed(() => {
    const hideMobile = [...empList.mobile]
    //隱碼手機號碼
})

//刪除員工
function delEmp(idx) {
    empList.value.splice(idx, 1)
}

//修改員工
function modifyEmp(idx) {
    alert('修改第 '+ idx + ' 筆資料')
    modalEmpIdx.value = idx
    setModalEmpData(idx)
    const myModal = new bootstrap.Modal(document.getElementById('myModal'))
    myModal.show()
}

//將資料帶入Modal
function setModalEmpData(idx) {
    if (idx != null) {
        //modalEmpData.value = empList.value[idx]
        //因為用v-model雙向綁定這個寫法會導致資料邊改變邊顯示在Modal上
        //所以改用以下寫法，這樣就不會有資料邊改變邊顯示在Modal上的問題
        //但要注意vue3 ref的特性，不能直接改變裡面的值，要用.value
        modalEmpData.value.empNo = empList.value[idx].empNo
        modalEmpData.value.name = empList.value[idx].name
        modalEmpData.value.email = empList.value[idx].email
        modalEmpData.value.mobile = empList.value[idx].mobile
    } else {
        //當idx為空白時，清空modalEmpData
        modalEmpData.value = { empNo: '', name: '', email: '', mobile: '' }
    }

}

//將資料存回empList
function saveEmpData() {
    if (modalEmpIdx.value != null) {
        empList.value[modalEmpIdx.value] = modalEmpData.value
    } else {
        empList.value.push(modalEmpData.value)
    }
    setModalEmpData(null)
    modalEmpData.value = { empNo: '', name: '', email: '', mobile: '' }
}

</script>

<style>

</style>