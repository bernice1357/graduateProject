<template lang="pug">
el-table(
  border
  :data="filterStaticData"
  :header-cell-style="headerStyle"
  size="large"
  height="500"
)
  el-table-column(
    prop="name"
    label="靜態資料欄位"
    align="center"
  )
  el-table-column(
    prop="value"
    align="center"
  )
    template(#header)
      el-input(
        v-model="staticSearch"
        clearable size="small" 
        placeholder="搜尋靜態欄位"
      )
</template>
<script>
import { computed, ref, onMounted } from 'vue'
import axios from 'axios'
export default {
  name: 'staticData',
  props: {
    id: String,
  },
  setup(props) {
    const staticData = ref([])
    const getData = async () => {
      staticData.value = await axios.post(
        ' https://vae.fly.dev/patient/lab/static/',
        { id: props.id }
      )
      staticData.value = staticData.value.data.data
    }
    const headerStyle = { background: '#395679', color: '#ffffff' }
    const staticSearch = ref('')
    const filterStaticData = computed(() =>
      staticData.value.filter(
        (data) =>
          !staticSearch.value ||
          data.name.toLowerCase().includes(staticSearch.value.toLowerCase())
      )
    )

    onMounted(() => {
      getData()
    })

    return {
      staticData,
      filterStaticData,
      headerStyle,
    }
  },
}
</script>
<style scoped>
.el-table--large {
  font-size: 20px;
}
</style>
