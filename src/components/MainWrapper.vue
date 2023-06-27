<template>
  <div class="wrapper">
    <div class="text-editor">
      <textarea
        :style="{
          minWidth: '200px',
          minHeight: '500px',
        }"
        v-model="inputData"/>
      <div v-if="!isJson && inputData.length" class="error">Введите json</div>
      <button @click="generateForm">Сгенерировать форму</button>
    </div>
    <div class="div" :style="{display: 'flex', flexDirection: 'column'}">
      <div class="modal">
        <div
          class="container"
        >
          <MainContainer
            @getCodeArray="getFlattenArray(currentArray)"
            :key="containerKey"
            :container-info="getAllParents"
            :check-length="getAllParents"
          />
        </div>
      </div>
      <div v-if="codeArray.length" class="code-array">
        {{codeArray}}
      </div>
    </div>
  </div>
</template>

<script>
import MainContainer from '@/components/MainContainer'

export default {
  name: 'MainWrapper',
  components: {
    MainContainer
  },
  data () {
    return {
      containerKey: 1,
      inputData: '',
      infinityArray: [],
      errorJson: false,
      isJson: false,
      lengthArray: null,
      numberOfContainers: [],
      pending: null,
      getAllParents: [],
      currentArray: [],
      codeArray: []
    }
  },
  methods: {
    getFlattenArray () {
      this.codeArray = []
      const tempArray = this.flatten(this.currentArray)
      // eslint-disable-next-line array-callback-return
      tempArray.map((item) => {
        this.codeArray.push({ [item.code]: item.value === undefined ? 'Значения нет' : item.value })
      })
    },
    flatten (array) {
      const flatten = [];
      (function flat (array) {
        array.forEach(function (el) {
          if (Array.isArray(el)) flat(el)
          else flatten.push(el)
        })
      })(array)
      return flatten
    },
    generateForm () {
      this.containerKey += 1
      this.pending = false
      this.getAllParents = []
      const allParents = []
      this.currentArray.forEach((i) => {
        if (i.type === 'container') {
          allParents.push(i)
        } else return false
      })
      // eslint-disable-next-line array-callback-return
      const getAllParents = allParents.map((container) => {
        const parentContainer = []
        // eslint-disable-next-line array-callback-return
        this.currentArray.map((item) => {
          if (container.code === item.parent) {
            parentContainer.push(item)
          }
        })
        return parentContainer
      })
      this.pending = true
      this.getAllParents = getAllParents
    },
    isJsonString (str) {
      try {
        JSON.parse(str)
        this.isJson = true
      } catch (e) {
        this.isJson = false
      }
    }
  },
  watch: {
    inputData: {
      immediate: true,
      handler () {
        this.isJsonString(this.inputData)
        if (this.isJson) {
          this.currentArray = JSON.parse(this.inputData)
        }
      }
    }
  }
}

</script>

<style scoped lang="scss">
.wrapper{
  display: flex;
  flex-direction: row;
  :nth-child(1n){
    margin-right: 20px;
    margin-bottom: 20px;
  }
  .text-editor{
    display: flex;
    flex-direction: column;
    :nth-child(1n){
      margin-bottom: 10px;
    }
  }
  .code-array{
    display: flex;
    flex-direction: row;
  }
}
.error{
  color: red;
}
</style>
