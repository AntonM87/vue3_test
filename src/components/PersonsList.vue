<template>
  <PersonItem
      @remove="$emit('remove',person)"
      v-for="person of persons"
      :key="person.id"
      :person="person"
      class="person-item"/>
</template>

<script>
import PersonItem from "@/components/PersonItem";

export default {
  name: "PersonsList",
  components: {PersonItem},
  props: {
    persons: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>
.person-item {
  border-radius: .25rem;
  border: 1px solid rgba(58, 196, 249, 1);
  margin-bottom: 1rem;
  padding: 1rem;
}
</style>