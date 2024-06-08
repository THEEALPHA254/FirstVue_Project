<script setup>
import { ref } from 'vue';
import AddMembers from '@/components/AddMembers.vue';
import MembersList from '@/components/MembersList.vue';

let all_members = ref([]);
const addmembersref = ref(null);

function addMember(member) {
  member.id = all_members.value.length + 1;
  all_members.value.push(member);
}

function updateMember(id, updatedMember) {
  const memberIndex = all_members.value.findIndex(member => member.id === id);
  if (memberIndex !== -1) {
    all_members.value[memberIndex] = { id, ...updatedMember };
  }
}

function deleteMember(id) {
  all_members.value = all_members.value.filter(member => member.id !== id);
}

function editMember(id) {
  const member = all_members.value.find(member => member.id === id);
  addmembersref.value.setEditMember(member);
}
</script>

<template>
  <main>
  <h1>REGISTRATION</h1>
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
