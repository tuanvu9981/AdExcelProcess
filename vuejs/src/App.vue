<template>
  <v-app>
    <v-app-bar app color="blue">
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/tuanvu0801junda/AdExcelProcess"
        target="_blank"
        text
      >
        <span class="mr-2">Github</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <!-- <HelloWorld/> -->
      <UploadFile
        @name-err="alertFileName"
        @size-err="alertFileSize"
        @ok-reply="oKReply"
        @err-reply="errReply"
        @no-file-err="noFileReply"
      />
      <div v-if="upAlertCtrl">
        <UploadAlert :errMessage="this.message" :fileName="this.fileName" />
      </div>
      <div v-if="downErrCtrl">
        <DownloadErrExcel :filename="this.fileName" />
      </div>

      <div v-if="dialog" class="text-center">
        <v-dialog v-model="dialog" width="500">
          <v-card>
            <v-card-title class="text-h5 grey lighten-2">
              Information
            </v-card-title>

            <v-card-text>
              Process completed! Data from <b>{{ this.fileName }}</b> was saved
              into database.
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text @click="finalClean()">
                Back to Main Page
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </v-main>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld';
import UploadFile from "./components/excelFileProcess/UploadFile";
import UploadAlert from "./components/excelFileProcess/UploadAlert";
import DownloadErrExcel from "./components/excelFileProcess/DownloadErrExcel";

export default {
  name: "App",

  components: {
    // HelloWorld,
    UploadFile,
    UploadAlert,
    DownloadErrExcel,
  },

  data() {
    return {
      dialog: false,
      upAlertCtrl: false,
      downErrCtrl: false,
      message: "",
      fileName: "",
    };
  },

  methods: {
    reset() {
      this.dialog = false;
      this.upAlertCtrl = false;
      this.downErrCtrl = false;
      this.message = "";
      this.fileName = null;
    },

    alertFileName(data) {
      this.reset();
      this.upAlertCtrl = true;
      this.message = data.str;
      this.fileName = data.fileName;
    },

    alertFileSize(data) {
      this.reset();
      this.upAlertCtrl = true;
      this.message = data.str;
      this.fileName = data.fileName;
    },

    noFileReply() {
      this.reset();
      this.upAlertCtrl = true;
      this.message = "No file selected ! Please choose an .xlsx file !";
    },

    oKReply(data) {
      this.reset();
      this.dialog = true;
      this.fileName = data.fileName;
    },

    errReply(data) {
      this.reset();
      this.downErrCtrl = true;
      this.fileName = data.fileName;
      // get file, click download --> download error.xlsx
    },

    finalClean() {
      this.dialog = false;
    },
  },
};
</script>
