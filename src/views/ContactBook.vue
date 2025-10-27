<!-- src/views/ContactBook.vue -->
<template>
  <div class="page">
    <div class="row">
      <div class="col-md-12">
        <InputSearch v-model="searchText" />
      </div>
    </div>

    <div class="row mt-3">
     
      <div class="col-md-6">
        <h4>
          Danh bạ
          <i class="fas fa-address-book"></i>
        </h4>

        <ContactList
          v-if="filteredContactsCount > 0"
          :contacts="filteredContacts"
          v-model:activeIndex="activeIndex"
        />
        <p v-else>Không có liên hệ nào.</p>

        <div class="mt-3 row justify-content-around align-items-center">
          <button class="btn btn-sm btn-primary" @click="refreshList()">
            <i class="fas fa-redo"></i> Làm mới
          </button>

          <button class="btn btn-sm btn-success" @click="goToAddContact">
            <i class="fas fa-plus"></i> Thêm mới
          </button>

          <button class="btn btn-sm btn-danger" @click="removeAllContacts">
            <i class="fas fa-trash"></i> Xóa tất cả
          </button>
        </div>
      </div>

     
      <div class="col-md-6 mt-3 mt-md-0">
        <div v-if="activeContact">
          <h4>
            Chi tiết Liên hệ
            <i class="fas fa-address-card"></i>
          </h4>
          <ContactCard :contact="activeContact" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ContactCard from "@/components/ContactCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ContactList from "@/components/ContactList.vue";
import ContactService from "@/services/contact.service";

export default {
  components: { ContactCard, InputSearch, ContactList },
  data() {
    return {
      contacts: [],
      activeIndex: -1,
      searchText: "",
    };
  },
  watch: {
    
    searchText() {
      this.activeIndex = -1;
    },
  },
  computed: {
   
    contactStrings() {
      return this.contacts.map(({ name, email, address, phone }) =>
        [name, email, address, phone].join("")
      );
    },
    
    filteredContacts() {
      if (!this.searchText) return this.contacts;
      return this.contacts.filter((_contact, index) =>
        this.contactStrings[index].includes(this.searchText)
      );
    },
    // Liên hệ đang được chọn
    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },
    // Số lượng liên hệ sau khi lọc
    filteredContactsCount() {
      return this.filteredContacts.length;
    },
  },
  methods: {
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retrieveContacts();
      this.activeIndex = -1;
    },
    async removeAllContacts() {
      if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
        try {
          await ContactService.deleteAll();
          this.refreshList();
        } catch (error) {
          console.log(error);
        }
      }
    },
    goToAddContact() {
      this.$router.push({ name: "contact.add" });
    },
  },
  mounted() {
    this.refreshList();
  },
};
</script>

<style scoped>
/* Thêm CSS tùy chỉnh nếu cần */
</style>
