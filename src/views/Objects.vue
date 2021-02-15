<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app>
      <v-sheet color="grey lighten-4" class="pa-4">
        <v-list-item class="px-2">
          <v-list-item-avatar>
            <v-img src="https://randomuser.me/api/portraits/men/85.jpg"></v-img>
          </v-list-item-avatar>

          <v-list-item-title>Demo User</v-list-item-title>
        </v-list-item>

        <div>demo@user.com</div>
      </v-sheet>

      <v-divider></v-divider>

      <v-list>
        <v-list-item
          v-for="[icon, text, link] in links"
          :key="icon"
          link
          v-on:click="$router.push(link)"
        >
          <v-list-item-icon>
            <v-icon>{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-icon>
            <v-list-item-title>Доступный баланс</v-list-item-title>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>500</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-icon>
            <v-list-item-title>Кредитный лимит</v-list-item-title>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>50</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <div class="px-3">
        <v-btn block color="blue" dark elevation="2">
          Пополнить баланс
        </v-btn>
      </div>
    </v-navigation-drawer>

    <v-main>
      <v-container class="py-8 px-6" fluid>
        <v-row>
          <v-col cols="12">
            <v-card>
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

                  <v-btn icon @click.stop="dialogAddObject = true">
                    <v-icon>mdi-plus</v-icon>
                  </v-btn>
                </v-toolbar>
                <div class="d-flex flex-wrap align-start mb-6">
                  <v-card
                    class="ma-3"
                    max-width="344"
                    outlined
                    @click.stop="openDialog(construction)"
                    v-for="construction in constructions"
                    :key="construction.id"
                  >
                    <v-list-item>
                      <v-list-item-content>
                        <v-list-item-title>{{
                          construction.Name
                        }}</v-list-item-title>
                        <v-list-item-subtitle></v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                    <v-list-item three-line>
                      <v-list-item-content>
                        <v-list-item-subtitle>
                          Дата: {{ construction.Start }}
                        </v-list-item-subtitle>
                        <v-list-item-subtitle>
                          Гид: {{ construction.guide.username }}
                        </v-list-item-subtitle>
                        <v-list-item-subtitle>
                          Подключений:
                          {{ construction.ListenersPlaned }} чел.
                        </v-list-item-subtitle>
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
                <v-dialog v-model="dialogDetails">
                  <v-card>
                    <v-card-title class="headline">
                      {{ openedConstruction.name }}
                    </v-card-title>
                    <v-row no-gutters>
                      <v-col cols="12" sm="6">
                        <v-card-text>
                          <GmapMap
                            :center="openedConstruction.center"
                            :zoom="17"
                            map-type-id="satellite"
                            style="width: 100%; height: 500px"
                          >
                            <GmapMarker
                              v-for="worker in openedConstruction.workers"
                              v-if="worker.position"
                              :position="worker.position.location"
                              :key="worker.position.id"
                            ></GmapMarker>
                            <GmapPolygon
                              :paths="openedConstruction.polygon"
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
                            >Адрес:
                            {{
                              openedConstruction.address
                            }}</v-list-item-subtitle
                          >
                          <v-list-item-subtitle
                            >Застройщик:
                            {{ openedConstruction.developerName }} /
                            {{
                              openedConstruction.developerINN
                            }}</v-list-item-subtitle
                          >
                          <v-list-item-subtitle
                            v-if="openedConstruction.workers"
                          >
                            Работников на объекте:
                            {{ openedConstruction.workers.length }} чел.
                          </v-list-item-subtitle>
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
                              <v-card-title>
                                <v-spacer></v-spacer>
                                <v-btn
                                  tile
                                  color="success"
                                  @click.stop="dialogAddWorker = true"
                                >
                                  <v-icon left>
                                    mdi-account-multiple-plus
                                  </v-icon>
                                  Добавить работника
                                </v-btn>
                              </v-card-title>
                              <v-data-table
                                :headers="workersHeaders"
                                :items="openedConstruction.workers"
                                :search="search"
                                item-key="name"
                                class="elevation-1"
                              >
                              </v-data-table>
                            </v-card>
                            <v-card color="basil" flat v-else>
                              <v-data-table
                                :headers="accidentsHeaders"
                                :items="openedConstruction.sos"
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

                      <v-btn
                        color="green darken-1"
                        text
                        @click="dialogDetails = false"
                      >
                        Закрыть
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
                <v-dialog
                  v-model="dialogAddWorker"
                  persistent
                  max-width="600px"
                >
                  <v-card>
                    <v-card-title>
                      <span class="headline">Профиль строителя</span>
                    </v-card-title>
                    <v-card-text>
                      <v-container>
                        <v-row>
                          <v-col cols="12">
                            <v-text-field
                              label="Фамилия Имя Отчество"
                              required
                              v-model="newWorker.fio"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              label="Email"
                              required
                              v-model="newWorker.email"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              label="Телефон"
                              required
                              v-model="newWorker.phone"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12">
                            <v-text-field
                              label="Password*"
                              type="password"
                              v-model="newWorker.password"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-text-field
                              label="Работодатель"
                              required
                              v-model="newWorker.employerName"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12" sm="6">
                            <v-autocomplete
                              :items="[
                                'Маляр-штукатур',
                                'Каменщик',
                                'Монтажник',
                                'Сварщик',
                                'Крановщик',
                              ]"
                              required
                              v-model="newWorker.occupation"
                              label="Специальность"
                            ></v-autocomplete>
                          </v-col>
                        </v-row>
                      </v-container>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="dialogAddWorker = false"
                      >
                        Отмена
                      </v-btn>
                      <v-btn color="blue darken-1" text @click="addNewWorker()">
                        Сохранить
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
                <v-dialog
                  v-model="dialogAddObject"
                  persistent
                  max-width="600px"
                >
                  <v-card>
                    <v-card-title>
                      <span class="headline"
                        >Создать новый объект строительства</span
                      >
                    </v-card-title>
                    <v-card-text>
                      <v-container>
                        <v-row>
                          <v-col cols="12">
                            <v-text-field
                              label="Название объекта"
                              required
                              v-model="newObject.name"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12">
                            <v-text-field
                              label="Адрес"
                              required
                              v-model="newObject.address"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12">
                            <v-text-field
                              label="Застройщик"
                              required
                              v-model="newObject.developerName"
                            ></v-text-field>
                          </v-col>
                          <v-col cols="12">
                            <GmapMap
                              :center="{ lat: 55.753, lng: 37.616 }"
                              :zoom="15"
                              map-type-id="satellite"
                              style="width: 100%; height: 300px"
                            >
                              <GmapPolygon
                                :paths="demoPath"
                                :editable="true"
                                :options="pathOptions"
                                @paths_changed="updateNew($event)"
                              >
                              </GmapPolygon>
                            </GmapMap>
                          </v-col>
                        </v-row>
                      </v-container>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="dialogAddObject = false"
                      >
                        Отмена
                      </v-btn>
                      <v-btn color="blue darken-1" text @click="addNewObject()">
                        Сохранить
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
const axios = require("axios");
const qs = require("qs");
export default {
  created: function () {
    axios
      .get("http://185.5.54.22:1077/excursions")
      .then((response) => (this.constructions = response.data));
  },
  methods: {
    openDialog: function (construction) {
      this.openedConstruction = construction;
      axios
        .get(
          "https://apistroinav.dic.li/users?construction=" +
            this.openedConstruction.id
        )
        .then((response) => (this.openedConstruction.workers = response.data));
      axios
        .get(
          "https://apistroinav.dic.li/sos?constructionId=" +
            this.openedConstruction.id
        )
        .then((response) => (this.openedConstruction.sos = response.data));
      this.dialogDetails = true;
    },
    updateNew(mvcArray) {
      let paths = [];
      for (let i = 0; i < mvcArray.getLength(); i++) {
        let path = [];
        for (let j = 0; j < mvcArray.getAt(i).getLength(); j++) {
          let point = mvcArray.getAt(i).getAt(j);
          path.push({ lat: point.lat(), lng: point.lng() });
        }
        paths.push(path);
        console.log(paths);
      }
      this.editedPath = paths;
    },
    updateEdited(mvcArray) {
      let paths = [];
      for (let i = 0; i < mvcArray.getLength(); i++) {
        let path = [];
        for (let j = 0; j < mvcArray.getAt(i).getLength(); j++) {
          let point = mvcArray.getAt(i).getAt(j);
          path.push({ lat: point.lat(), lng: point.lng() });
        }
        paths.push(path);
        console.log(paths);
      }
      this.editedPath = paths;
      this.uploadNewPath(paths);
    },
    uploadNewPath(paths) {
      var data = qs.stringify({ polygon: this.editedPath[0] });
      var config = {
        method: "put",
        url: "https://apistroinav.dic.li/constructions/1",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded",
        },
        data: data,
      };

      axios(config)
        .then(function (response) {
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    addNewObject() {
      var data = qs.stringify({
        name: this.newObject.name,
        address: this.newObject.address,
        developerName: this.newObject.developerName,
        center: { lat: 55.753, lng: 37.616 },
        polygon: this.editedPath[0],
      });
      var config = {
        method: "post",
        url: "https://apistroinav.dic.li/constructions/",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded",
        },
        data: data,
      };
      var self = this;
      axios(config)
        .then(function (response) {
          self.dialogAddWorker = false;
          self.dialogDetails = false;
          self.dialogAddObject = false;
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    addNewWorker() {
      var data = qs.stringify({
        fio: this.newWorker.fio,
        username: this.newWorker.fio,
        email: this.newWorker.email,
        password: this.newWorker.password,
        employerName: this.newWorker.employerName,
        phone: this.newWorker.phone,
        occupation: this.newWorker.occupation,
        construction: this.openedConstruction.id,
        confirmed: true,
      });
      var config = {
        method: "post",
        url: "https://apistroinav.dic.li/users/",
        headers: {
          "Content-Type": "application/x-www-form-urlencoded",
        },
        data: data,
      };
      var self = this;
      axios(config)
        .then(function (response) {
          self.dialogAddWorker = false;
          self.dialogDetails = false;
          console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
  data() {
    return {
      newWorker: {},
      newObject: {},
      editedPath: null,
      constructions: [],
      openedConstruction: {},
      search: null,
      workersHeaders: [
        { text: "ФИО", align: "start", value: "fio" },
        { text: "Специальность", align: "start", value: "occupation" },
        { text: "Работодатель", align: "start", value: "employerName" },
        { text: "Телефон", value: "phone" },
      ],
      accidentsHeaders: [
        { text: "ФИО", align: "start", value: "uID.fio" },
        { text: "Специальность", align: "start", value: "uID.occupation" },
        { text: "Телефон", value: "uID.phone" },
        { text: "Время инцидента", align: "start", value: "created_at" },
        { text: "Статус", align: "start", value: "type" },
      ],
      tab: null,
      items: ["Список субъектов", "Журнал инцидентов"],
      dialogDetails: false,
      dialogAddWorker: false,
      dialogAddObject: false,
      demoPath: [
        [
          { lat: 55.755, lng: 37.614 },
          { lat: 55.755, lng: 37.618 },
          { lat: 55.751, lng: 37.618 },
          { lat: 55.751, lng: 37.614 },
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