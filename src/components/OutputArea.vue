<template>
  <div>
    <v-switch
      v-model="sortSwitch"
      class="mx-auto"
      max-width="800"
      elevation="2"
      :label="sortSwitch ? '一致率順' : '一致数順'"
    />
    <v-card
      v-for="result_data in sortSwitch ? rateOrderDatas : countOrderDatas"
      :key="result_data.departmentName"
      elevation="2"
      class="o-area mx-auto mb-1"
      :class="(theme ? 'darken' : 'lighten') + '-4'"
      :color="
        DEPARTMENT_TABLE.filter((dt) => {
          return dt.key == result_data.departmentName;
        })[0].school.color
      "
      max-width="800"
    >
      <v-card-title primary-title>
        {{
          DEPARTMENT_TABLE.filter((dt) => {
            return dt.key == result_data.departmentName;
          })[0].name
        }}
      </v-card-title>
      <v-card-text>
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <span v-bind="attrs" v-on="on"
              >一致率: {{ Math.round(result_data.result.rate * 100) }}%
            </span>
          </template>
          <span>
            選択された学びたい分野、学びたくない分野と学科（コース）で学べること、学ばないことの一致率
          </span>
        </v-tooltip>
        <span>,</span>
        <span>一致数: {{ result_data.result.count }}/{{ result_data.result.all }}</span>
      </v-card-text>
      <v-chip
        class="ma-2"
        v-for="matchedIndex in result_data.result.matchedFieldIndexes"
        :key="matchedIndex"
        >{{ ALL_FIELD_LIST[matchedIndex] }}</v-chip
      >
    </v-card>
  </div>
</template>

<script>
import { ALL_FIELD_LIST, DEPARTMENT_TABLE } from "../fieldTable.js";
export default {
  name: "OutputArea",
  props: {
    result_datas: [],
    theme: Boolean,
  },
  data() {
    return {
      ALL_FIELD_LIST: ALL_FIELD_LIST,
      DEPARTMENT_TABLE: DEPARTMENT_TABLE,
      sortSwitch: true,
    };
  } /*
  watch: {
    result_datas: function () {
      console.log(this.result_datas);
      this.result_datas.forEach((result_data, index) => {
        console.log(DEPARTMENT_TABLE[index].key, result_data.departmentName);
        console.log(DEPARTMENT_TABLE[index].key === result_data.departmentName);
        console.log(
          DEPARTMENT_TABLE.filter((dt) => {
            return dt.key == result_data.departmentName;
          })
        );
      });
    },
  },
  created: function () {
    console.log("created");
    console.log(this.ALL_FIELD_LIST);
    console.log(this.result_datas, this.sortSwitch);
  },*/,
  computed: {
    rateOrderDatas() {
      console.log(
        this.result_datas
          .slice()
          .sort((a, b) => b.result.rate - a.result.rate)
          .map((obj) => obj.result.rate)
      );
      return this.result_datas.slice().sort((a, b) => b.result.rate - a.result.rate);
    },
    countOrderDatas() {
      console.log(
        this.result_datas
          .slice()
          .sort((a, b) => b.result.count - a.result.count)
          .map((obj) => obj.result.count)
      );
      return this.result_datas.slice().sort((a, b) => b.result.count - a.result.count);
    },
  },
};
</script>
