<template>
  <el-table :data="list" style="width: 100%;padding-top: 15px;">
    <el-table-column v-slot="scope" label="Order_No" min-width="200">
      {{ scope.row.order_no | orderNoFilter }}
    </el-table-column>
    <el-table-column v-slot="scope" label="Price" width="195" align="center">
      ¥{{ scope.row.price | toThousandFilter }}
    </el-table-column>
    <el-table-column v-slot="{ row }" label="Status" width="100" align="center">
      <el-tag :type="row.status | statusFilter">
        {{ row.status }}
      </el-tag>
    </el-table-column>
  </el-table>
</template>

<script>
import { transactionList } from '@/api/remote-search'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 30)
    }
  },
  data() {
    return {
      list: null
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      transactionList().then((response) => {
        this.list = response.data.items.slice(0, 8)
      })
    }
  }
}
</script>
