<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import AddMembers from '@/components/AddMembers.vue';
import MembersList from '@/components/MembersList.vue';

let all_members = ref([]);
const addmembersref = ref(null);

const api = axios.create({
  baseURL: 'http://localhost:8000/api/',
  headers: {
    'Content-Type': 'application/json'
  }
});

async function fetchMembers() {
  try {
    const response = await api.get('members/');
    all_members.value = response.data;
  } catch (error) {
    console.error('Error fetching members:', error);
  }
}
async function addMember(member) {
  try {
    const response = await api.post('members/', member);
    fetchMembers()
  } catch (error) {
    console.error('Error adding member:', error);
  }
}

async function updateMember(id, updatedMember) {
  try {
    const response = await api.put(`member/${id}/`, updatedMember);
    fetchMembers()
   
  } catch (error) {
    console.error('Error updating member:', error);
  }
}

async function deleteMember(id) {
  try {
    await api.delete(`member/${id}/`);
    all_members.value = all_members.value.filter(member => member.id !== id);
  } catch (error) {
    console.error('Error deleting member:', error);
  }
}

function editMember(id) {
  const member = all_members.value.find(member => member.id === id);
  addmembersref.value.setEditMember(member);
}

onMounted(()=>{
  fetchMembers()
})

</script>

<template>
  <main>
  <h1 >REGISTRATION FORM</h1>
    <div class="row mt-4">
      <div class="col-12 col-md-6 offset-md-3">
        <div class="card">
          <div class="card-body">
            <div class="alert alert-warning text-center" v-if="all_members.length == 0">
                 No members registered. 
            </div>

              <div v-else>
                <MembersList :all_members="all_members" @deleteMember="deleteMember" @editMember="editMember" />
              </div>
             
                <AddMembers ref="addmembersref" @addMember="addMember" @updateMember="updateMember" />
              
              
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
