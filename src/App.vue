<template>
  <main>
    <h1>English Class</h1>
    <h3>Here is an overview from the english class</h3>
    <CustomTable 
      :columns="tableColumns"
      :rows="students"
    >
      <!-- Custom formating to email column -->
      <template #email="slotProps">
        <EmailLink :email="slotProps.field" />
      </template>

    </CustomTable>
  </main>
</template>

<script>
import CustomTable from "@/components/CustomTable.vue";
import EmailLink from "@/components/EmailLink.vue";
import { students } from '@/database/students.js' 

const tableColumns = {
  email: {
    name: 'Email',
    sort: 'string'
  },
  firstName: {
    name: 'First Name',
    sort: 'string'
  },
  lastName: {
    name: 'Last Name',
    sort: 'string'
  },
  group: {
    name: 'Primary Group',
    sort: 'string'
  },
  phoneNumber: {
    name: 'Phone Number',
  },
  hours: {
    name: 'Hours Studied',
    sort: 'number'
  }
}

export default {
  components: {
    CustomTable,
    EmailLink
  },
  data() {
    return {
      tableColumns,
      students
    }
  }
}
</script>