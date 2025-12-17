<template>
    <div class="mt-4">
        <h3 ></h3>
        <table class="table table-bordered table-hover mt-2">
            <thead>
                <tr>
                    <th>Mã thú cưng</th>
                    <th>Tên thú cưng</th>
                    <th>Loài</th>
                    <th>Tuổi</th>
                    <th>Cân nặng (kg)</th>
                    <th>Tiêm chủng</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="pet in props.pets" :key="pet.id" @click="onSelectPet(pet)" style="cursor: pointer;">
                    <td>{{ pet.id }}</td>
                    <td>{{ pet.name }}</td>
                    <td>{{ pet.type }}</td>
                    <td>{{ pet.age }}</td>
                    <td>{{ pet.weight }}</td>
                    <td>{{ pet.vaccinated ? 'Đã tiêm' : 'Chưa tiêm' }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
    import { defineEmits, defineProps } from 'vue';
    const props = defineProps({
        pets: {
            type: Array,
            required: true,
        }
    });

    const emit = defineEmits(['select-pet'])
    const onSelectPet = (pet) => {
        emit('select-pet', {...pet});
    };
</script>