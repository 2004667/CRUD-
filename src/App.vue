<script>
export default {
  name: 'App',
  data() {
    return {
      users: JSON.parse(localStorage.getItem('users')) || [
        { title: 'Arman', description: 'Employed' },
        { title: 'Asiya', description: 'Employed' },
        { title: 'Erlan', description: 'Unemployed' },
        { title: 'Jahan', description: 'Employed' },
        { title: 'Fatima', description: 'Unemployed' },
        { title: 'Nurayim', description: 'Employed' },
        { title: 'Kenes', description: 'Unemployed' },
        { title: 'Nursultan', description: 'Unemployed' },
        { title: 'Gibrat', description: 'Employed' },
        { title: 'Era', description: 'Employed' },
        { title: 'Aisha', description: 'Unemployed' },
        { title: 'Dias', description: 'Unemployed' },
      ],
      create: false ,
      edit: false,
      newUser: {
        title: '',
        description: ''
      },
      editUserIndex: null
    };
  },
  methods: {
    saveUsersToLocalStorage() {
      localStorage.setItem('users', JSON.stringify(this.users));
    },
    toggleDiv() {
      this.create = !this.create;
      this.edit = false;
      this.clearInputs();
    },
    addUser() {
      if (this.newUser.title && this.newUser.description) {
        this.users.push({ ...this.newUser });
        this.saveUsersToLocalStorage();
        this.clearInputs();
        this.create = false;
      } else {
        alert("Please fill in both fields.");
      }
    },
    showEditUser(index) {
      this.editUserIndex = index;
      this.newUser = { ...this.users[index] };
      this.edit = true;
      this.create = false;
    },
   editUser() {
      const updatedUser = { ...this.users[this.editUserIndex] };
      
      if (this.newUser.title) {
        updatedUser.title = this.newUser.title;
      }
      if (this.newUser.description) {
        updatedUser.description = this.newUser.description;
      }

      if (this.newUser.title || this.newUser.description) {
        this.users.splice(this.editUserIndex, 1, updatedUser);
        this.saveUsersToLocalStorage();
        this.clearInputs();
        this.edit = false;
      } else {
        alert("Please fix at least one field.");
      }
    },
    deleteUser(index) {
      this.users.splice(index, 1);
      this.saveUsersToLocalStorage();
    },
    clearInputs() {
      this.newUser.title = '';
      this.newUser.description = '';
    },
    cancel() {
      this.create = false;
      this.edit = false;
      this.clearInputs();
    }
  }
};
</script>
<template>
  <div>
    <h1 class="text-[30px] flex justify-center mt-[3%]">Admin Panel</h1>
    <div class="flex justify-center mt-[5%]">
      <div class="bg-[#FFFFFF] w-[600px] h-[50%] flex flex-col justify-start rounded-md drop-shadow-md">
        <div class="bg-[#000000] drop-shadow-sm w-full h-[50px] rounded-md flex items-center px-4">
          <div class="w-3/12 text-[#ffffff]">Number</div>
          <div class="w-3/12 text-[#ffffff]">Title</div>
          <div class="w-4/12 text-[#ffffff]">Description</div>
          <div class="w-2/12  text-[#ffffff]">Actions</div>
        </div>
        <div v-for="(user, index) in users" :key="index" class="border-b border-[#000000] w-full h-[50px] flex items-center px-4">
          <div class="w-3/12">{{ index + 1 }}</div>
          <div class="w-3/12">{{ user.title }}</div>
          <div class="w-4/12">{{ user.description }}</div>
          <div class="w-2/12 flex justify-end">
            <button @click="showEditUser(index)" class="w-[60px] h-[24px] bg-[#B5DCFF] rounded-md text-[#FFFFFF]">Update</button>
            <button @click="deleteUser(index)" class="w-[60px] h-[24px] bg-[#E02923] rounded-md text-[#FFFFFF] ml-[5px]">Delete</button>
          </div>
        </div>
      </div>
    </div>
    <div class="flex justify-center">
      <button class="bg-[#3CCE2E] w-[100px] h-[40px] rounded-md text-[#FFFFFF] active:bg-[#2F8227] hover:bg-[#2F8227] mt-[50px]" @click="toggleDiv">Create</button>
    </div>
    <div class="flex justify-center">
      <div v-if="create" class="w-[400px] h-[260px] bg-[#FFFFFF] rounded-md drop-shadow-lg mt-[-600px]">
        <div class="flex justify-center mt-[3%]">
          <h1>Please enter the following parameters</h1>
        </div>
        <div class="flex items-center justify-center mt-[8%]">
          <input v-model="newUser.title" type="text" class="w-[250px] h-[40px] bg-[#FFFFFF] border-[3px] border-[#000000] rounded-md" placeholder="title..." id="titlecreate">
        </div>
        <div class="flex items-center justify-center mt-[5%]">
          <input v-model="newUser.description" type="text" class="w-[250px] h-[40px] bg-[#FFFFFF] border-[3px] border-[#000000] rounded-md" placeholder="info..." id="infocreate">
        </div>
        <div class="flex justify-end space-x-2 mt-[13%] text-[#FFFFFF]">
          <button class="w-[60px] h-[20px] bg-[#B5DCFF] rounded-md" id="ok" @click="addUser">Ok</button>
          <button class="w-[60px] h-[20px] bg-[#E02923] rounded-md" id="cancel" @click="cancel">Cancel</button>
        </div>
      </div>

      <div v-if="edit" class="w-[400px] h-[260px] bg-[#FFFFFF] rounded-md drop-shadow-lg mt-[-600px]">
        <div class="flex justify-center mt-[3%]">
          <h1>Edit the following parameters</h1>
        </div>
        <div class="flex items-center justify-center mt-[8%]">
          <input v-model="newUser.title" type="text" class="w-[250px] h-[40px] bg-[#FFFFFF] border-[3px] border-[#000000] rounded-md" placeholder="title..." id="titlecreate">
        </div>
        <div class="flex items-center justify-center mt-[5%]">
          <input v-model="newUser.description" type="text" class="w-[250px] h-[40px] bg-[#FFFFFF] border-[3px] border-[#000000] rounded-md" placeholder="info..." id="infocreate">
        </div>
        <div class="flex justify-end space-x-2 mt-[13%] text-[#FFFFFF]">
          <button class="w-[60px] h-[20px] bg-[#B5DCFF] rounded-md" id="ok" @click="editUser">Ok</button>
          <button class="w-[60px] h-[20px] bg-[#E02923] rounded-md" id="cancel" @click="cancel">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>


