<template lang="pug">
  #app
    h1.title {{ title }}
    input(type="text" placeholder="Enter Tasks..." @keyup.enter.prevent="addTask").input-task
    .radio-action
      input(type="radio" v-model="filterType" value="all")#all
      label(for="all") ALL
      input(type="radio" v-model="filterType" value="done")#done
      label(for="done") DONE
      input(type="radio" v-model="filterType" value="working")#working
      label(for="working") WORKING
    
    transition-group(name="fade-list" tag="ul" class="flexboxs")
      li(v-for="(task, index) in filterItems" :key="task.name" :class="{done: task.done}").item-list
        .normal-text(v-if="!task.edit")
          p {{ task.name }}
        .edit-text(v-else)
          input(type="text" :value="task.name" @keyup.enter.prevent="updateTask($event, index)").edit-input
        button(@click.prevent="editTask(index)" v-if="!task.edit && !task.done") edit
        button(@click.prevent="doneTask(index)" v-show="!task.edit && !task.done") done
        button(@click.prevent="doneTask(index)" v-show="task.done") undo
        button(@click.prevent="removeTask(index)") remove

</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      title: 'Vue Todos',
      tasks: [],
      filterType: 'all',
    }
  },
  methods: {
    addTask(e) {
      const newTask = { 
        name: e.target.value.trim(), 
        done: false, 
        edit: false ,
      }
      this.tasks.push(newTask)
      e.target.value = ''
      this.filterType = 'all'
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    },
    doneTask(index) {
      this.tasks[index].done =! this.tasks[index].done
    },
    editTask(index) {
      this.tasks[index].edit = true
    },
    updateTask(e,index) {
      this.tasks[index].name = e.target.value.trim()
      this.tasks[index].edit = false
    }
  },
  computed: {
    filterItems() {
      let filterData = []
      switch(this.filterType) {
        case 'done':
          filterData = this.tasks.filter(res => res.done === true)
          console.log(filterData)
          break
        case 'working':
          filterData = this.tasks.filter(res => res.done === false)
          break
        default:
          filterData = this.tasks
      }
      return filterData
    }
  }
}
</script>

<style lang="sass">
*
  box-sizing: border-box

body
  background: #ccc
  font-size: 18px

#app
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin: 0px auto
  margin-top: 60px
  width: 60%
  .title
    letter-spacing: 5px
    border-left: 10px solid rgba(red, 0.5)
    background: #fff
    padding: 10px
    box-shadow: 0px 20px 20px -10px rgba(#333 , 0.5)
  .input-task
    font-size: 1.5rem
    padding: 10px
    width: 100%
    border: none
    border-bottom: 5px inset rgba(blue , 0)
    outline: none
    text-align: center
    transition: all 0.5s 
    &:focus
      border-bottom: 5px inset rgba(blue , 0.5)
  
  .radio-action
    margin-top: 20px
    color: #333
    
    input[type="radio"]
      display: none
    
    label::before
      content: ''
      display: inline-block
      width: 20px
      height: 20px
      position: relative
      top: 3px
      margin: 0px 10px
      background: #fff
      border-radius: 50%
    input[type="radio"]:checked + label::before
      background: #333

  .flexboxs
    display: flex
    flex-direction: column
    margin: 0px
    padding: 0px
    position: relative
    .fade-list-enter
      opacity: 0
      transform: translateY(55px)
    .fade-list-leave-to
      opacity: 0
      transform: translateY(-55px)
    .fade-list-leave-active
      position: absolute
      width: 100%
    
    .item-list
      list-style: none
      background: #fff
      margin: 15px 0px
      padding: 5px 10px
      border-left: 10px solid rgba(green , 0.5)
      font-size: 1.2rem
      display: flex
      align-items: center
      transition: all 1s
      &.done
        border-color: #2c2c2c
        background: rgba(#333 , 0.5)
        p
          text-decoration: line-through
      
      .normal-text
        flex: 1
        text-align: left
      .edit-text
        flex: 1
        text-align: left
        .edit-input
          width: 100%
          font-size: 1.3rem
          border: none
          border-bottom: 1px solid #333
          padding: 10px
          outline: none

button
  background: none
  border: none
  border: 1px solid rgba(#333 , 0.5)
  padding: 10px
  font-size: 1rem
  margin-left: 10px
  height: 50px
  color: #333
  cursor: pointer
  transition: all 0.5s
  text-transform: uppercase
  outline: none
  &:hover
    border: 1px solid rgba(#333 , 0)
    background: #333
    color: #fff  
</style>
