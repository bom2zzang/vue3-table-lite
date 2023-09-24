<template>
  <table-lite
    :is-slot-mode="true"
    :is-row-detail="true"
    :is-loading="table.isLoading"
    :columns="table.columns"
    :rows="table.rows"
    :total="table.totalRecordCount"
    :sortable="table.sortable"
    @do-search="doSearch"
    @is-finished="table.isLoading = false"
  >
    <template v-slot:id="data">
      <button @click="showDetail(data.value.id)">{{ data.value.id }}</button>
    </template>
    <template v-slot:name="data">
      <Test>
        {{ data.value.name }}
      </Test>
    </template>

    <!--    상세 화면 설정 예시 -->
    <template v-slot:rowDetail="data">
      <RowDetailView :data="data.value.id" />
    </template>
  </table-lite>
</template>

<script>
import { defineComponent, reactive } from "vue";
import TableLite from "../components/TableLite.vue";
import Test from "../components/Test.vue";
import RowDetailView from "@/examples/RowDetailView.vue";

// Fake Data for 'asc' sortable
const sampleData1 = (offst, limit) => {
  offst = offst + 1;
  let data = [];
  for (let i = offst; i <= limit; i++) {
    data.push({
      id: i,
      name: "TEST" + i,
      email: "test" + i + "@example.com",
      rowDetail: [{ item: 1 }, { item: 9 }, { item: 2 }],
    });
  }
  return data;
};

// Fake Data for 'desc' sortable
const sampleData2 = (offst, limit) => {
  let data = [];
  for (let i = limit; i > offst; i--) {
    data.push({
      id: i,
      name: "TEST" + i,
      email: "test" + i + "@example.com",
      rowDetail: [{ item: 1 }, { item: 9 }, { item: 2 }],
    });
  }
  return data;
};

export default defineComponent({
  name: "App",
  components: { TableLite, Test, RowDetailView },
  setup() {
    // Table config
    const table = reactive({
      isLoading: false,
      columns: [
        {
          label: "ID",
          field: "id",
          width: "3%",
          sortable: true,
          isKey: true,
        },
        {
          label: "Name",
          field: "name",
          width: "10%",
          sortable: true,
        },
        {
          label: "Email",
          field: "email",
          width: "15%",
          sortable: true,
        },
      ],
      rows: [],
      totalRecordCount: 0,
      sortable: {
        order: "id",
        sort: "asc",
      },
    });

    /**
     * Search Event
     */
    const doSearch = (offset, limit, order, sort) => {
      table.isLoading = true;
      setTimeout(() => {
        table.isReSearch = offset == undefined ? true : false;
        if (offset >= 10 || limit >= 20) {
          limit = 20;
        }
        if (sort == "asc") {
          table.rows = sampleData1(offset, limit);
        } else {
          table.rows = sampleData2(offset, limit);
        }
        table.totalRecordCount = 20;
        table.sortable.order = order;
        table.sortable.sort = sort;
      }, 600);
    };

    // First get data
    doSearch(0, 10, "id", "asc");

    const showDetail = (id) => {
      table.rows = table.rows.map((v) => {
        if (v.id === id) {
          v.detailShow = !v.detailShow;
        }
        return v;
      });
    };

    return {
      table,
      doSearch,
      showDetail,
    };
  },
});
</script>
