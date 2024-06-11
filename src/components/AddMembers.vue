<template>
  <form action="" @submit="submitForm">
    <div>
      <div class="row">
        <p v-if="editing_mode">Edit clicked Member</p>
        <div class="col-6">
          <input type="text" v-model="username" placeholder="John Doe" class="form-control" required>
        </div>
        <div class="col-6">
          <input type="tel" v-model="contact" placeholder="0712-345-678" class="form-control" required>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-6">
          <input type="text" v-model="department" placeholder="Department" class="form-control">
        </div>
        <div class="col-6">
          <input type="email" v-model="email" placeholder="example@gmail.com" class="form-control">
        </div>
      </div>
      <div class="d-grid gap-2 mt-4">
        <button class="btn btn-primary" :class="editing_mode ? 'btn-warning' : 'btn-primary'" type="submit">
          {{ editing_mode ? 'Update' : 'Add Member' }}
        </button>
        <button v-if="editing_mode" class="btn btn-dark mt-2" @click="discardEdit">Cancel</button>
      </div>
    </div>
  </form>
  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  
  const emit = defineEmits(['addMember', 'updateMember']);
  
  let username = ref('');
  let contact = ref('');
  let department = ref('');
  let email = ref('');
  let editing_mode = ref(false);
  let selectedMember = ref(null);
  
  function submitForm(e) {

    e.preventDefault()
    if (!username.value || !contact.value || !department.value || !email.value) {
      return alert('Please fill out all fields');
    }
  
    const member = {
      username: username.value,
      contact: contact.value,
      department: department.value,
      email: email.value
    };
  
    if (editing_mode.value) {
      emit('updateMember', selectedMember.value.id, member);
      editing_mode.value = false;
      selectedMember.value = null;
    } else {
      emit('addMember', member);
    }
  
    resetForm();
  }
  
  function setEditMember(member) {
    username.value = member.username;
    contact.value = member.contact;
    department.value = member.department;
    email.value = member.email;
    selectedMember.value = member;
    editing_mode.value = true;
  }
  
  function discardEdit() {
    resetForm();
    editing_mode.value = false;
    selectedMember.value = null;
  }
  
  function resetForm() {
    username.value = '';
    contact.value = '';
    department.value = '';
    email.value = '';
  }
  
  defineExpose({
    setEditMember
  });
  </script>
  