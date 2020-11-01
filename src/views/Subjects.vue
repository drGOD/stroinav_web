<template>
  <v-data-table
    :headers="dessertHeaders"
    :items="desserts"
    :search="search"
    single-expand="true"
    :expanded.sync="expanded"
    item-key="name"
    show-expand
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Список субъектов</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Поиск"
          single-line
          hide-details
        ></v-text-field>
      </v-toolbar>
    </template>
    <template v-slot:expanded-item="">
      <GmapMap
        :center="center"
        :zoom="17"
        map-type-id="satellite"
        style="width: 600px; height: 500px"
      >
        <GmapMarker
          v-for="m in markers"
          :position="m.position"
          :key="m.position.lat"
          :clickable="true"
          :draggable="true"
          @click="center = m.position"
        ></GmapMarker>
        <GmapPolygon
          :paths="paths"
          :editable="true"
          :options="pathOptions"
          @paths_changed="updateEdited($event)"
        >
        </GmapPolygon>
      </GmapMap>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data() {
    return {
      center: {
        lat: 55.857726,
        lng: 37.691607,
      },
      markers: [
        {
          position: {
            lat: 55.857971,
            lng: 37.689412,
          },
        },
      ],
      paths: [
        [
          { lat: 55.857058, lng: 37.688616 },
          { lat: 55.857318, lng: 37.68868 },
          { lat: 55.857676, lng: 37.689699 },
          { lat: 55.85798, lng: 37.690407 },
          { lat: 55.857584, lng: 37.691099 },
          { lat: 55.858606, lng: 37.693129 },
          { lat: 55.858869, lng: 37.692708 },
          { lat: 55.859065, lng: 37.692437 },
          { lat: 55.858964, lng: 37.692249 },
          { lat: 55.859647, lng: 37.691179 },
          { lat: 55.85778, lng: 37.687443 },
        ],
        [
          { lat: 1.382, lng: 103.802 },
          { lat: 1.382, lng: 103.808 },
          { lat: 1.388, lng: 103.808 },
          { lat: 1.388, lng: 103.802 },
        ],
      ],
      pathOptions: {
        fillOpacity: 0.5,
        strokeColor: "#00FF00",
        strokeOpacity: 0.2,
        strokeWeight: 2,
        fillColor: "#00ff00",
      },
      search: "",
      expanded: [],
      dessertHeaders: [
        { text: "ФИО", align: "start", value: "name" },
        { text: "Специальность", align: "start", value: "occupation" },
        { text: "Работодатель", align: "start", value: "company" },
        { text: "Телефон", value: "phone" },
      ],
      desserts: [
        {
          name: "Петров Петр Петрович",
          occupation: "Маляр",
          company: 'ООО "МСУ-1"',
          phone: "+79998887766",
        },
        {
          name: "Иванов Иван Иванович",
          occupation: "Штукатурщик",
          company: 'ООО "МСУ-1"',
          phone: "+79998887766",
        },
        {
          name: "Семенов Семен Семенович",
          occupation: "Каменщик",
          company: 'ООО "Строймонтаж"',
          phone: "+79998887766",
        },
        {
          name: "Кузьмин Кузьма Кузьмич",
          occupation: "Каменщик",
          company: 'ООО "Стройсервис"',
          phone: "+79998887766",
        },
        {
          name: "Никитин Никита Никитич",
          occupation: "Сварщик",
          company: 'ООО "Строймонтаж"',
          phone: "+79998887766",
        },
        {
          name: "Дмитриев Дмитрий Дмитревич",
          occupation: "Монтажник",
          company: 'ООО "МСУ-1"',
          phone: "+79998887766",
        },
      ],
    };
  },
  methods: {
    updateEdited(mvcArray) {
      let paths = [];
      for (let i = 0; i < mvcArray.getLength(); i++) {
        let path = [];
        for (let j = 0; j < mvcArray.getAt(i).getLength(); j++) {
          let point = mvcArray.getAt(i).getAt(j);
          path.push({ lat: point.lat(), lng: point.lng() });
        }
        paths.push(path);
      }
      this.edited = paths;
    },
  },
};
</script>