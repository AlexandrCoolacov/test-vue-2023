<template>
  <section v-if="containerInfo.length">
    <div
      class="container"
    >
      <div
        v-for="(item, idx) of containerInfo[0]"
        :key="idx"
      >
        <input v-if="item && item.type === 'input'" type="text">
        <select  v-if="item && item.type === 'list'" name="" id="">
          <option
            v-for="(option,i) in item.listdata"
            :key="i"
          >
            {{option.value}}
          </option>
        </select>
        <input v-if="item && item.type === 'datepicker'" type="date">
      </div>
      <MainContainer
        v-if="containerInfo.length"
        :container-info="changedContainerInfo"
      />
    </div>
    <button
      @click="getCodeArray"
      v-if="checkLength  && (containerInfo.length === checkLength.length)"
    >
      Получить массив значений
    </button>
  </section>
</template>

<script>

export default {
  name: 'MainContainer',
  props: {
    containerInfo: {
      required: false
    },
    checkLength: null
  },
  created () {
    this.changedContainerInfo = [...this.containerInfo]
    console.log(this.changedContainerInfo.shift())
  },
  data () {
    return {
      changedContainerInfo: []
    }
  },
  methods: {
    getCodeArray () {
      this.$emit('getCodeArray')
    }
  },
  watch: {
    containerInfo: {
      immediate: true,
      deep: true,
      handler () {
        if (this.containerInfo && this.containerInfo.length > 1) {
          this.changedContainerInfo = [...this.containerInfo]
          this.changedContainerInfo.slice(1)
          console.log(this.changedContainerInfo)
        } else return false
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container{
  border: 2px solid gray;
  display: flex;
  flex-direction: column;
  padding: 10px;
  :nth-child(1n) {
    margin-bottom: 10px;
  }
}

</style>
