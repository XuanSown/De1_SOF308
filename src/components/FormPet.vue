<template>
    <div class="card p-3">
        <h4 class="card-title">Thông tin thú cưng</h4>
        <form @submit.prevent="savePet">
            <div class="mb-3">
                <label class="form-label">Mã thú cưng</label>
                <input type="text" class="form-control" v-model="formPet.id" :disabled="isEditing" required>
            </div>
            <div class="mb-3">
                <label class="form-label">Tên thú cưng</label>
                <input type="text" class="form-control" v-model="formPet.name" required>
            </div>
            <div class="mb-3">
                <label class="form-label">Loài</label>
                <select class="form-control" v-model="formPet.type">
                    <option value="Chó">Chó</option>
                    <option value="Mèo">Mèo</option>
                    <option value="Chuột">Chuột</option>
                    <option value="Chim">Chim</option>
                </select>
            </div>
            <div class="mb-3">
                <label class="form-label">Tuổi</label>
                <input type="number" class="form-control" v-model="formPet.age" min="0">
            </div>
            <div class="mb-3">
                <label class="form-label">Cân nặng (kg)</label>
                <input type="number" step="0.1" class="form-control" v-model="formPet.weight" min="0.1">
            </div>
            <div class="mb-3">
                <label class="form-label d-block">Tiêm chủng</label>
                <div class="form-check">
                    <input class="form-check-input" type="radio" :value="true" v-model="formPet.vaccinated">
                    <label class="form-check-label">Đã tiêm</label>
                </div>
                <div class="form-check">
                    <input class="form-check-input" type="radio" :value="false" v-model="formPet.vaccinated">
                    <label class="form-check-label">Chưa tiêm</label>
                </div>
            </div>
            <div class="d-flex">
                <button type="button" class="btn btn-primary" @click="savePet">Save</button>
                <button type="button" class="btn btn-danger" @click="removePet">Delete</button>
            </div>
        </form>
    </div>
</template>

<script setup>
import { reactive, ref, watch } from 'vue';

const props = defineProps({
    petSelected: {
        type: Object,
        default: null
    }
});

//nếu có dữ liệu đang sửa -> khóa id
const isEditing = ref(false);

const emit = defineEmits('save-pet', 'remove-pet');

const formPet = reactive({
    id: '',
    name: '',
    type: 'Chó',
    age: 0,
    weight: 0,
    vaccinated: false
});

const resetForm = () => {
        formPet.id ='';
        formPet.name = '';
        formPet.type = 'Chó';
        formPet.age = 0;
        formPet.weight = 0;
        formPet.vaccinated = false
        isEditing.value = false;
};

watch(() => props.petSelected, (newPet) => {
    if (newPet) {
        Object.assign(formPet, newPet);
        isEditing.value = true;
    } else {
        resetForm();
    }
});



const savePet = () => {
    if (!formPet.id || !formPet.name) {
        alert("Vui lòng nhập đầy đủ thông tin");
        return;
    }
    emit('save-pet', { ...formPet })
};

const removePet = () => {
    if (!formPet.id) {
        alert("Chọn thú cưng để xóa");
        return;
    }
    if (confirm('Bạn có chắc muốn xóa')) {
        emit('remove-pet', formPet.id);
    }
};

</script>