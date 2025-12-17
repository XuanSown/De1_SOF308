<template>
    <div class="card p-3">
        <h4 class="card-title">Thông tin thú cưng</h4>
        <form @submit.prevent="savePet">
            <div class="mb-3">
                <label class="form-label">Mã thú cưng</label>
                <input type="text" class="form-control" v-model="props.petSelected.id" :disabled="isEditing" required>
            </div>
            <div class="mb-3">
                <label class="form-label">Tên thú cưng</label>
                <input type="text" class="form-control" v-model="props.petSelected.name" required>
            </div>
            <div class="mb-3">
                <label class="form-label">Loài</label>
                <select class="form-control" v-model="props.petSelected.type">
                    <option value="Chó">Chó</option>
                    <option value="Mèo">Mèo</option>
                    <option value="Chuột">Chuột</option>
                    <option value="Chim">Chim</option>
                </select>
            </div>
            <div class="mb-3">
                <label class="form-label">Tuổi</label>
                <input type="number" class="form-control" v-model="props.petSelected.age" min="0">
            </div>
            <div class="mb-3">
                <label class="form-label">Cân nặng (kg)</label>
                <input type="number" step="0.1" class="form-control" v-model="props.petSelected.weight" min="0.1">
            </div>
            <div class="mb-3">
                <label class="form-label d-block">Tiêm chủng</label>
                <div class="form-check">
                    <input class="form-check-input" type="radio" :value="true" v-model="props.petSelected.vaccinated">
                    <label class="form-check-label">Đã tiêm</label>
                </div>
                <div class="form-check">
                    <input class="form-check-input" type="radio" :value="false" v-model="props.petSelected.vaccinated">
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
import { reactive } from 'vue';

const props = defineProps({
    petSelected: {
        type: Object,
        default: null
    }
});

const emit = defineEmits(['save-pet', 'remove-pet']);

const savePet = () => {
    if (!props.petSelected.id || !props.petSelected.name) {
        alert("Vui lòng nhập đầy đủ thông tin");
        return;
    }
    emit('save-pet', { ...props.petSelected })
};

const removePet = () => {
    if (!props.petSelected.id) {
        alert("Chọn thú cưng để xóa");
        return;
    }
    if (confirm('Bạn có chắc muốn xóa')) {
        emit('remove-pet', props.petSelected.id);
    }
};

</script>