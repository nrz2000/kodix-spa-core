<template lang="pug">
  section.services
    v-container(style="height: 100%" v-if="services")
      v-row(style="height: 20%")
        v-col(cols="3")
          v-autocomplete(
            v-model="location"
            :items="services"
            label="Магазин"
            item-text="location"
            item-value="location"
            outlined
            clearable
          )
      v-row(style="height: 80%")
        BaseTable(
          :data="filteredServices"
          :headers="tableHeaders"
          :columnsWithSorting="['payment_date']"
          @sort="sortServices"
        )
    v-container(
      v-else
      style="height: 100%"
      class="text-center d-flex align-center justify-center"
    )
      v-progress-circular(
        size="100"
        indeterminate
        color="primary"
      )
</template>

<script>
import BaseTable from '@/components/BaseTable.vue';

export default {
  components: {
    BaseTable,
  },
  data() {
    const tableHeaders = {
      id: 'ID',
      location: 'Магазины',
      service: 'Сервисы',
      uuid: 'UUID',
      request: 'Заявка',
      payment_date: 'Дата платежа',
      slot_date: 'Дата слота',
      answer_code: 'Код ответа',
      service_message: 'Сообщение сервиса',
    };
    return {
      location: null,
      services: null,
      tableHeaders,
    };
  },
  created() {
    import('@/assets/services.json').then((res) => { this.services = res.default; });
  },
  computed: {
    filteredServices() {
      if (!this.location) return this.services;
      return this.services.filter((s) => s.location === this.location);
    },
  },
  methods: {
    sortServices(type, key) {
      if (key === 'payment_date') {
        const parsingData = (data) => new Date(data[key]).getTime();
        if (type === 'ascending') {
          this.services.sort((a, b) => parsingData(a) - parsingData(b));
        } else {
          this.services.sort((a, b) => parsingData(b) - parsingData(a));
        }
      }
      if (type === 'ascending') {
        this.services.sort((a, b) => a[key] - b[key]);
      } else {
        this.services.sort((a, b) => b[key] - a[key]);
      }
    },
  },
};
</script>

<style lang="sass">
  .services
    height: 100%
    padding: 30px 0
</style>
