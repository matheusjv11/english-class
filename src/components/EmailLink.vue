<template>
  <a :href="`mailto:${email}`">{{ email }}</a>
</template>

<script>
export default {
    props: {
        email: {
            type: String,
            required: true
        }
    }
}
</script>