<template>
  <div ref="content" class="hz-m-wrap">
    <van-image class="hz-u-img" :width="width" :height="height" :src="myItem.propValues[imageIndex].value" />
    <ul class="hz-m-area" v-add-item>
      <zone
        class="hz-m-item"
        v-for="(zone, index) in zones"
        :key="index"
        :index="index"
        :setting="zone"
        @delItem="removeItem($event)"
        @changeInfo="changeInfo($event)"
      ></zone>
    </ul>
  </div>
</template>

<script>
import Zone from './Zone'
import addItem from '../directives/addItem'

export default {
  name: 'HotZone',
  data() {
    return {
      zones: [],
      imageIndex: 0,
      widthIndex: 1,
      heightIndex: 2
    }
  },
  computed: {
    width: function () {
      return this.myItem.propValues[this.widthIndex].value.num + this.myItem.propValues[this.widthIndex].value.unit
    },
    height: function () {
      return this.myItem.propValues[this.heightIndex].value.num + this.myItem.propValues[this.heightIndex].value.unit
    }
  },
  props: {
    myItem: {}
    // image: {
    //   type: String,
    //   required: true
    // },
    // zonesInit: {
    //   type: Array,
    //   default: () => []
    // },
    // max: {
    //   type: Number
    // }
  },
  mounted() {
    this.zones = this.myItem.zonesInit.concat()
  },
  methods: {
    changeInfo(res) {
      let { info, index } = res

      this.changeItem(info, index)
    },
    addItem(setting) {
      this.zones.push(setting)
      this.hasChange()
      this.$emit('add', setting)
    },
    eraseItem(index = this.zones.length - 1) {
      this.removeItem(index)
      this.$emit('erase', index)
    },
    isOverRange() {
      let { max, zones } = this.myItem

      return max && zones.length > max
    },
    overRange() {
      const index = this.zones.length - 1

      this.removeItem(index)
      this.$emit('overRange', index)
    },
    removeItem(index = this.zones.length - 1) {
      this.zones.splice(index, 1)
      this.hasChange()
      this.$emit('remove', index)
    },
    changeItem(info, index = this.zones.length - 1) {
      Object.assign(this.zones[index], info)
      this.hasChange()
    },
    hasChange() {
      this.$emit('change', this.zones)
    }
  },
  directives: {
    addItem
  },
  components: {
    Zone
  }
}
</script>

<style scoped>
@import '../assets/styles/main.css';
</style>
