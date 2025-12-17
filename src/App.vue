<script setup>
import { ref } from 'vue';
import FormPet from './components/FormPet.vue';
import TablePet from './components/TablePet.vue';

const pets = ref([
    { id: 'PET01', name: 'Micky', type: 'Chó', age: 2, weight: 5.5, vaccinated: true },
    { id: 'PET02', name: 'Mimi', type: 'Mèo', age: 1, weight: 3.2, vaccinated: false }
]);

const petSelected = ref(null);
const handleSelectPet = (pet) => {
    petSelected.value = { ...pet }
}

const handleSavePet = (formData) => {
    const index = pets.findIndex(p => p.id === formData.id);

    if (index != -1) {
        //tồn tại -> cập nhật
        pets[index] = formData;
    } else {
        //0 tồn tại -> thêm mới
        pets.push(formData);
    }

    petSelected.value = null
}

const handleRemovePet = (petId) => {
    const index = pets.findIndex(p => p.id === petId);
        if (index !== -1) {
            pets.splice(index, 1);
        }
        petSelected.value = null;
}
</script>

<template>
    <div class="container mt-4">
        <h1 class="text-center">Quản lí thú cưng</h1>
        <FormPet :pet-selected="petSelected" @save-pet="handleSavePet" @remove-pet="handleRemovePet" />
        <hr>
        <TablePet :pets="pets" @select-pet="handleSelectPet" />

        <div class="text-center" v-if="pets.length == 0">
            Danh sách trống!
        </div>
    </div>
</template>