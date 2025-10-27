<script>
export default {
  props: {
    contact: { type: Object, required: true },
  },
};
</script>

<template>
  <div>
    <div class="p-1">
      <strong>Tên:</strong>
      {{ contact.name }}
    </div>

    <div class="p-1">
      <strong>E-mail:</strong>
      {{ contact.email }}
    </div>

    <div class="p-1">
      <strong>Địa chỉ:</strong>
      {{ contact.address }}
    </div>

    <div class="p-1">
      <strong>Điện thoại:</strong>
      {{ contact.phone }}
    </div>

    <div class="p-1">
      <strong>Liên hệ yêu thích:&nbsp;</strong>
      <i v-if="contact.favorite" class="fas fa-check"></i>
      <i v-else class="fas fa-times"></i>
    </div>
  </div>
  <ContactCard :contact="activeContact" />
<router-link
:to="{
name: 'contact.edit',
params: { id: activeContact._id },
}"
>
<span class="mt-2 badge badge-warning">
<i class="fas fa-edit"></i> Hiệu chỉnh</span
>
</router-link>
</template>
