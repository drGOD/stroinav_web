<template>
  <div>
    <v-toolbar>
      <v-text-field
        hide-details
        prepend-icon="mdi-magnify"
        single-line
      ></v-text-field>
      <v-btn icon>
        <v-icon>mdi-filter</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-table</v-icon>
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-toolbar>
    <div class="d-flex flex-wrap align-start mb-6">
      <v-card class="ma-3" max-width="344" outlined @click.stop="dialog = true">
        <v-list-item>
          <v-list-item-avatar>
            <v-img src="https://mgsu.ru/kod/MGSUrui751.png"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>Реконструкция территрии</v-list-item-title>
            <v-list-item-subtitle></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item three-line>
          <v-list-item-content>
            <v-list-item-subtitle
              >Адрес: СВАО, Ярославское шоссе, вл. 26</v-list-item-subtitle
            >
            <v-list-item-subtitle>Застройщик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle>Ген.подрядчик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle
              >Субъектов на объекте: 112 чел.</v-list-item-subtitle
            >
            <v-list-item-subtitle
              >Иницидентов на объекте: 15 шт. (+1)</v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
        <v-card-actions>
          <v-btn outlined rounded text>
            <v-icon>mdi-clipboard-list</v-icon> Журнал
          </v-btn>
          <v-btn outlined rounded text>
            <v-icon>mdi-fountain-pen-tip</v-icon>Редактировать
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-card class="ma-3" max-width="344" outlined @click.stop="dialog = true">
        <v-list-item>
          <v-list-item-avatar>
            <v-img src="https://mgsu.ru/kod/MGSUrui751.png"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>Строительство парковки</v-list-item-title>
            <v-list-item-subtitle></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item three-line>
          <v-list-item-content>
            <v-list-item-subtitle
              >Адрес: СВАО, Ярославское шоссе, вл. 26</v-list-item-subtitle
            >
            <v-list-item-subtitle>Застройщик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle>Ген.подрядчик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle
              >Субъектов на объекте: 24 чел.</v-list-item-subtitle
            >
            <v-list-item-subtitle
              >Иницидентов на объекте: 0 шт.</v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
        <v-card-actions>
          <v-btn outlined rounded text>
            <v-icon>mdi-clipboard-list</v-icon> Журнал
          </v-btn>
          <v-btn outlined rounded text>
            <v-icon>mdi-fountain-pen-tip</v-icon>Редактировать
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-card class="ma-3" max-width="344" outlined @click.stop="dialog = true">
        <v-list-item>
          <v-list-item-avatar>
            <v-img src="https://mgsu.ru/kod/MGSUrui751.png"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>Благоустройство пруда</v-list-item-title>
            <v-list-item-subtitle></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item three-line>
          <v-list-item-content>
            <v-list-item-subtitle
              >Адрес: СВАО, Ярославское шоссе, вл. 26</v-list-item-subtitle
            >
            <v-list-item-subtitle>Застройщик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle>Ген.подрядчик: НИУ МГСУ</v-list-item-subtitle>
            <v-list-item-subtitle
              >Субъектов на объекте: 41 чел.</v-list-item-subtitle
            >
            <v-list-item-subtitle
              >Иницидентов на объекте: 2 шт.</v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
        <v-card-actions>
          <v-btn outlined rounded text>
            <v-icon>mdi-clipboard-list</v-icon> Журнал
          </v-btn>
          <v-btn outlined rounded text>
            <v-icon>mdi-fountain-pen-tip</v-icon>Редактировать
          </v-btn>
        </v-card-actions>
      </v-card>
    </div>
    <v-dialog v-model="dialog">
      <v-card>
        <v-card-title class="headline">
          Реконструкция территрии НИУ МГСУ
        </v-card-title>
        <v-row no-gutters>
          <v-col cols="12" sm="6">
            <v-card-text>
              <GmapMap
                :center="center"
                :zoom="17"
                map-type-id="satellite"
                style="width: 100%; height: 500px"
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
            </v-card-text>
          </v-col>
          <v-col cols="12" sm="6">
            <v-card-text>
              <v-list-item-subtitle
                >Адрес: СВАО, Ярославское шоссе, вл. 26</v-list-item-subtitle
              >
              <v-list-item-subtitle>Застройщик: НИУ МГСУ</v-list-item-subtitle>
              <v-list-item-subtitle
                >Ген.подрядчик: НИУ МГСУ</v-list-item-subtitle
              >
              <v-list-item-subtitle
                >Субъектов на объекте: 41 чел.</v-list-item-subtitle
              >
              <v-list-item-subtitle
                >Иницидентов на объекте: 2 шт.</v-list-item-subtitle
              >
            </v-card-text>
            <v-tabs
              v-model="tab"
              background-color="transparent"
              color="basil"
              grow
            >
              <v-tab v-for="item in items" :key="item">
                {{ item }}
              </v-tab>
            </v-tabs>

            <v-tabs-items v-model="tab">
              <v-tab-item v-for="item in items" :key="item">
                <v-card color="basil" flat v-if="tab == 0">
                  <v-data-table
                    :headers="dessertHeaders"
                    :items="desserts"
                    :search="search"
                    item-key="name"
                    class="elevation-1"
                  >
                  </v-data-table>
                </v-card>
                <v-card color="basil" flat v-else>
                  <v-data-table
                    :headers="accidentsHeaders"
                    :items="accidents"
                    :search="search"
                    item-key="name"
                    class="elevation-1"
                  >
                  </v-data-table>
                </v-card>
              </v-tab-item>
            </v-tabs-items>
          </v-col>
        </v-row>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialog = false">
            Disagree
          </v-btn>

          <v-btn color="green darken-1" text @click="dialog = false">
            Agree
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
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
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Иванов Иван Иванович",
          occupation: "Штукатурщик",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Семенов Семен Семенович",
          occupation: "Каменщик",
          company: 'ООО "Копыта и рога"',
          phone: "+79998887766",
        },
        {
          name: "Кузьмин Кузьма Кузьмич",
          occupation: "Каменщик",
          company: 'ООО "Копыта и рога"',
          phone: "+79998887766",
        },
        {
          name: "Никитин Никита Никитич",
          occupation: "Сварщик",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Дмитриев Дмитрий Дмитревич",
          occupation: "Монтажник",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
      ],
      accidentsHeaders: [
        { text: "ФИО", align: "start", value: "name" },
        { text: "Специальность", align: "start", value: "occupation" },
        { text: "Работодатель", align: "start", value: "company" },
        { text: "Телефон", value: "phone" },
      ],
      accidents: [
        {
          name: "Петров Петр Петрович",
          occupation: "Маляр",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Иванов Иван Иванович",
          occupation: "Штукатурщик",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Семенов Семен Семенович",
          occupation: "Каменщик",
          company: 'ООО "Копыта и рога"',
          phone: "+79998887766",
        },
        {
          name: "Кузьмин Кузьма Кузьмич",
          occupation: "Каменщик",
          company: 'ООО "Копыта и рога"',
          phone: "+79998887766",
        },
        {
          name: "Никитин Никита Никитич",
          occupation: "Сварщик",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
        {
          name: "Дмитриев Дмитрий Дмитревич",
          occupation: "Монтажник",
          company: 'ООО "Рога и копыта"',
          phone: "+79998887766",
        },
      ],
      tab: null,
      items: ["Список субъектов", "Журнал инцидентов"],
      text:
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
      dialog: false,
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
        {
          position: {
            lat: 55.858579,
            lng: 37.689779,
          },
        },
        {
          position: {
            lat: 55.858925,
            lng: 37.690584,
          },
        },
        {
          position: {
            lat: 55.858708,
            lng: 37.69207,
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
    };
  },
};
</script>