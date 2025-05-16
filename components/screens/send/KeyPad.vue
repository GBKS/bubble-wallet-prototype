<script setup>

const emit = defineEmits(['select'])

const keys = [{
    id: '1',
    number: 1,
    label: '1'
  },
  {
    id: '2',
    number: 2,
    label: '2'
  },
  {
    id: '3',
    number: 3,
    label: '3'
  },
  {
    id: '4',
    number: 4,
    label: '4'
  },
  {
    id: '5',
    number: 5,
    label: '5'
  },
  {
    id: '6',
    number: 6,
    label: '6'
  },
  {
    id: '7',
    number: 7,
    label: '7'
  },
  {
    id: '8',
    number: 8,
    label: '8'
  },
  {
    id: '9',
    number: 9,
    label: '9'
  },
  {
    id: 'point',
    label: '.'
  },
  {
    id: '0',
    number: 0,
    label: '0'
  },
  {
    id: 'back',
    icon: 'caretLeft'
}]

const select = (info) => {
  emit('select', info)
}

</script>

<template>
    <div class="send-key-pad">
      <ScreensSendKey
        v-for="info in keys"
        :key="info.id"
        :info="info"
        @click="select(info)"
      />
    </div>
</template>

<style scoped lang="scss">

.send-key-pad {
  display: flex;
  flex-wrap: wrap;
  gap: 2px;
}

</style>
