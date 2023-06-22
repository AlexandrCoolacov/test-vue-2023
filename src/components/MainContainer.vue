<template>
  <div
    v-if="containerInfo.length"
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
      {{ item.code }}
    </div>
    <MainContainer
      v-if="containerInfo.length"
      :container-info="changedContainerInfo"
    />
  </div>

</template>

<script>

export default {
  name: 'MainContainer',
  props: {
    containerInfo: {
      required: false
    }
  },
  created () {
    this.changedContainerInfo = [...this.containerInfo]
    console.log(this.changedContainerInfo.shift())
    console.log(this.changedContainerInfo)
    console.log(this.containerInfo)
  },
  data () {
    return {
      changedContainerInfo: []
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
