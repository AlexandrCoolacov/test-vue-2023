<template>
  <div class="wrapper">
    <div class="text-editor">
      <textarea v-model="inputData"/>
      <div v-if="!isJson" class="error">Введенный текст не соответствует формату json</div>
      <button @click="generateForm">Сгенерировать форму</button>
    </div>
    <div class="modal">
      <div
        class="container"
      >
        <MainContainer
          :key="containerKey"
          :container-info="getAllParents"
        />
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
      endArray: [],
      pending: null,
      getAllParents: [],
      testCurrentArray: []
    }
  },
  methods: {
    generateForm () {
      this.containerKey += 1
      this.pending = false
      this.getAllParents = []
      this.endArray = []
      let allParents = []
      const nestingLevel = []
      this.testCurrentArray.forEach((i) => {
        if (i.type === 'container') {
          nestingLevel.push(i)
          console.log(nestingLevel)
        } else return false
        console.log(i.type)
        // nestingLevel.push(i.parent)
        console.log(i.parent)
      })
      allParents = nestingLevel
      console.log(allParents)
      // eslint-disable-next-line array-callback-return
      const getAllParents = allParents.map((container) => {
        const parentContainer = []
        // eslint-disable-next-line array-callback-return
        this.testCurrentArray.map((item) => {
          if (container.code === item.parent) {
            parentContainer.push(item)
          }
        })
        console.log(parentContainer)
        this.endArray.push(parentContainer)
        return parentContainer
      })
      console.log(getAllParents)
      this.pending = true
      this.getAllParents = getAllParents
    },
    isJsonString (str) {
      try {
        JSON.parse(str)
        this.isJson = true
        console.log('это джсон')
      } catch (e) {
        this.isJson = false
        console.log('это не джсон')
      }
    }
  },
  computed: {
    // computedForm () {
    //   if (this.testCurrentArray) {
    //     let allParents = []
    //     const nestingLevel = []
    //     this.testCurrentArray.forEach((i) => {
    //       if (i.type === 'container') {
    //         nestingLevel.push(i)
    //       }
    //       console.log(i.type)
    //       // nestingLevel.push(i.parent)
    //       console.log(i.parent)
    //     })
    //     allParents = [...new Set(nestingLevel)]
    //     console.log(allParents)
    //     // eslint-disable-next-line array-callback-return
    //     const getAllParents = allParents.map((container) => {
    //       const parentContainer = []
    //       // eslint-disable-next-line array-callback-return
    //       this.testCurrentArray.map((item) => {
    //         if (container.code === item.parent) {
    //           parentContainer.push(item)
    //         }
    //       })
    //       console.log(parentContainer)
    //       this.endArray.push(parentContainer)
    //       return parentContainer
    //     })
    //     console.log(getAllParents)
    //     // eslint-disable-next-line vue/no-side-effects-in-computed-properties
    //     this.pending = true
    //     return getAllParents
    //   } return false
    // },
    // numberOfContainers () {
    //   const containers = []
    //   if (this.currentArray.length) {
    //     // eslint-disable-next-line array-callback-return
    //     this.currentArray.map(i => {
    //       if (i.type === 'container') {
    //         containers.push(i)
    //       }
    //     })
    //     return containers
    //   } return false
    // },
    currentArray () {
      if (this.isJson) {
        console.log('работает компутед')
        return JSON.parse(this.inputData)
      } return false
    }
  },
  watch: {
    inputData: {
      immediate: false,
      deep: true,
      handler () {
        this.isJsonString(this.inputData)
        console.log('1111')
      }
    },
    isJson: {
      deep: true,
      handler () {
        if (this.isJson) {
          console.log('работает компутед')
          this.testCurrentArray = JSON.parse(this.inputData)
          console.log(this.testCurrentArray)
        }
      }
    }

  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.wrapper{
  display: flex;
  flex-direction: row;
}
.error{
  color: red;
}
</style>

<!--<template>-->
<!--  <div class="wrapper">-->
<!--    <div class="text-editor">-->
<!--      <textarea v-model="inputData"/>-->
<!--      <div v-if="errorJson" class="error">Введите json</div>-->
<!--      <button @click="generateForm">Сгенерировать форму</button>-->
<!--    </div>-->
<!--    <div class="modal">-->
<!--      <div-->
<!--        class="container"-->
<!--      >-->
<!--        <Container/>-->
<!--      </div>-->
<!--    </div>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--import Container from '@/components/Container'-->

<!--export default {-->
<!--  name: 'MainContainer',-->
<!--  components: {-->
<!--    Container-->
<!--  },-->
<!--  data () {-->
<!--    return {-->
<!--      inputData: null,-->
<!--      infinityArray: [],-->
<!--      errorJson: false,-->
<!--      currentArray: []-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    generateForm () {-->
<!--      // eslint-disable-next-line no-unused-vars-->
<!--      const infinityArray = null-->
<!--      this.currentArray.forEach((item) => {-->
<!--        if (item.parent === null) {-->
<!--          console.log('Это первые контейнеры')-->
<!--        }-->
<!--      })-->
<!--      console.log('Форма сгенерирована')-->
<!--    }-->
<!--  },-->
<!--  watch: {-->
<!--    inputData: {-->
<!--      immediate: false,-->
<!--      deep: true,-->
<!--      inputDataHandler () {-->
<!--        try {-->
<!--          JSON.parse(this.inputData)-->
<!--          console.log(JSON.parse(this.inputData))-->
<!--          this.currentArray = JSON.parse(this.inputData)-->
<!--          console.log(this.currentArray)-->
<!--          // const nestingLevel = []-->
<!--          // this.currentArray.forEach((i) => {-->
<!--          //   console.log(parseInt(i.parent))-->
<!--          // })-->
<!--          //-->
<!--          // this.infinityArray = this.currentArray.flat(Infinity).join(' ')-->
<!--          // console.log(this.infinityArray)-->
<!--        } catch (e) {-->
<!--          this.errorJson = true-->
<!--        }-->
<!--        this.errorJson = false-->
<!--      }-->
<!--    }-->

<!--  }-->
<!--}-->

<!--</script>-->

<!--&lt;!&ndash; Add "scoped" attribute to limit CSS to this component only &ndash;&gt;-->
<!--<style scoped lang="scss">-->
<!--.wrapper{-->
<!--  display: flex;-->
<!--  flex-direction: row;-->
<!--}-->
<!--</style>-->
