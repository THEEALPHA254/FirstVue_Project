<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  all_members: Array
});

const emit = defineEmits(['deleteMember', 'editMember']);

function deleteMember(id) {
  // Emit delete event to parent
  emit('deleteMember', id);
}

function editMember(id) {
  console.log('edit member', id);

  // Emit edit event to parent
  emit('editMember', id);
}
</script>


<template>
    <div class="card my-3" v-for="member in all_members" :key="member.id">
        <div class="card-body">
            <div class="d-flex justify-content-between align-items-center">
                <span>{{ member.username }} | {{ member.contact }} | {{ member.department }} | {{ member.email }}</span>
                <div>
                    <i class="fa-solid fa-edit me-4" @click="editMember(member.id)"></i>
                    <i class="fa-solid fa-trash " @click="deleteMember(member.id)"></i>
                </div>

            </div>
        </div>
    </div>
</template>

<style lang="">

</style>