<template>
  <va-card>
    <h4 slot="header" class="card-title">
      Create New Event
    </h4>
    <br />
    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Title :</h5></b>
      </div>
      <div class="col-md-9">
        <!-- <div class="form-group"> -->
        <input
          type="text"
          v-model="model.EVENT_TITLE"
          class="form-control form-control-md"
        />
        <!-- </div> -->
      </div>
    </div>
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Content :</h5></b>
      </div>
      <div id="app" class="col-md-9">
        <vue-editor v-model="model.EVENT_CONTENT"></vue-editor>
      </div>
    </div>
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Document :</h5></b>
      </div>
      <div id="app" class="col-md-9">
        <VueFileAgent
          ref="vueFileAgent"
          :theme="'list'"
          :multiple="true"
          :deletable="true"
          :meta="true"
          :linkable="true"
          :accept="'.pdf,.xls,.xlsx'"
          :maxSize="'10MB'"
          :helpText="'Choose document'"
          :errorText="{
            type: 'Invalid file type. Only pdf or doc, docx or ods Allowed',
            size: 'Files should not exceed 10MB in size'
          }"
          @select="filesSelected($event)"
          @beforedelete="onBeforeDelete($event)"
          @delete="fileDeleted($event)"
          v-model="fileRecords"
        ></VueFileAgent>
      </div>
    </div>
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Start Date :</h5></b>
      </div>
      <div class="col-md-9">
        <!-- <div class="form-group"> -->
        <!-- <p v-if="id != undefined" class="text-left">{{title}}</p> -->
        <datepicker
          placeholder="dd-MM-yyyy"
          format="dd-MM-yyyy"
          input-class="form-control form-control-md bgcolor"
          v-model="model.EVENT_DATE_START"
          :typeable="false"
          name="uniquename"
        ></datepicker>
        <!-- </div> -->
      </div>
    </div>
    <br />
    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">End Date :</h5></b>
      </div>
      <div class="col-md-9">
        <datepicker
          placeholder="dd-MM-yyyy"
          format="dd-MM-yyyy"
          input-class="form-control form-control-md bgcolor"
          v-model="model.EVENT_DATE_END"
          :typeable="false"
          name="uniquename"
        ></datepicker>
        <!-- </div> -->
      </div>
    </div>
    <br />
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Set Distributor Audience Category :</h5></b>
      </div>
      <div class="col-md-9">
        <div class="row">
          <div class="col-4" v-for="(item, index) in compType" :key="index">
            <div class="row mb-2">
              <va-checkbox class="col-2" v-model="item.selected" />
              <span class="col mt-2 description">
                {{ item.DIST_TYPE_NAME }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br />
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Set Consultant Audience Category :</h5></b>
      </div>
      <div class="col-md-9">
        <div class="row">
          <div
            class="col-4"
            v-for="(item, index) in consultantType"
            :key="index"
          >
            <div class="row mb-2">
              <va-checkbox class="col-2" v-model="item.selected" />
              <span class="col mt-2 description">
                {{ item.TYPE_NAME }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br />
    <br />

    <div class="row">
      <div class="col-md-3">
        <b><h5 class="card-title">Set Other Audience Category :</h5></b>
      </div>
      <div class="col-md-9">
        <div class="row">
          <div class="col-4" v-for="(item, index) in otherType" :key="index">
            <div class="row mb-2">
              <va-checkbox class="col-2" v-model="item.selected" />
              <span class="col mt-2 description">
                {{ item.SET_PARAM }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <br />
    <br />
    <button
      @click="back"
      type="button"
      class="float-left btn btn-primary btn-fill btn-md"
    >
      <i class="fa fa-step-backward" /> &nbsp; Back
    </button>
    <div class="ml-2 float-right">
      <button
        @click="saveAsDraft"
        type="button"
        class="mr-2 btn btn-info btn-fill btn-md"
      >
        <i class="fa fa-pencil-square-o" /> &nbsp; Save as draft
      </button>
      <b-button
        @click="submit"
        type="button"
        class="btn btn-primary btn-fill btn-md"
      >
        <i class="fa fa-paper-plane" /> &nbsp; Publish
      </b-button>
    </div>
    <!-- <br />
                <br />
                <br />
                <br /> -->
  </va-card>
</template>

<script>
import { BootstrapVue, IconsPlugin } from "bootstrap-vue";

// Import Bootstrap an BootstrapVue CSS files (order is important)
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
import { debounce } from "lodash";
import moment from "moment";
import * as servicesModule0 from "../../../app/module0/services";
export default {
  mounted() {
    //this.getAllEvent();
    this.getDistributorType();
    this.getConsultantType();
    this.getOthersAudience();
  },
  methods: {
    async getOthersAudience() {
      const response = await servicesModule0.getOtherType();
      this.otherType = response;
    },
    async getDistributorType() {
      const response = await servicesModule0.getDistributorType();
      this.compType = response;
    },
    async getConsultantType() {
      const response = await servicesModule0.getConsultantType();
      this.consultantType = response;
    },
    async getAllEvent() {
      const response = await servicesModule0.getAllEvent();
      console.log("get all event : " + JSON.stringify(response));
    },
    selectAudience() {
      console.log("asdasd");
    },

    async saveAsDraft() {
      console.log(this.uploadFileOject);
      this.model.EVENT_DISTRIBUTOR_AUDIENCE = [];
      this.compType.forEach(item => {
        if (item.selected == true) {
          this.model.EVENT_DISTRIBUTOR_AUDIENCE.push(item.DISTRIBUTOR_TYPE_ID);
        }
      });
      this.model.EVENT_CONSULTANT_AUDIENCE = [];
      this.consultantType.forEach(item => {
        if (item.selected == true) {
          this.model.EVENT_CONSULTANT_AUDIENCE.push(item.CONSULTANT_TYPE_ID);
        }
      });
      this.model.EVENT_OTHER_AUDIENCE = [];
      this.otherType.forEach(item => {
        if (item.selected == true) {
          this.model.EVENT_OTHER_AUDIENCE.push(item.SETTING_GENERAL_ID);
        }
      });
      const data = new FormData();
      for (let i = 0; i < this.uploadFileOject.length; i++) {
        data.append("FILEOBJECT[]", this.uploadFileOject[i]);
      }
      data.append("ANNOUNCEMENT_STATUS", 1);
      data.append("EVENT_TITLE", this.model.EVENT_TITLE);
      data.append("EVENT_CONTENT", this.model.EVENT_CONTENT);
      data.append(
        "EVENT_DATE_START",
        moment(this.model.EVENT_DATE_START).format("YYYY-MM-DD")
      );
      data.append(
        "EVENT_DATE_END",
        moment(this.model.EVENT_DATE_END).format("YYYY-MM-DD")
      );
      if (this.model.EVENT_DISTRIBUTOR_AUDIENCE.length != 0) {
        data.append(
          "EVENT_DISTRIBUTOR_AUDIENCE",
          JSON.stringify(this.model.EVENT_DISTRIBUTOR_AUDIENCE)
        );
      }
      if (this.model.EVENT_CONSULTANT_AUDIENCE.length != 0) {
        data.append(
          "EVENT_CONSULTANT_AUDIENCE",
          JSON.stringify(this.model.EVENT_CONSULTANT_AUDIENCE)
        );
      }
      if (this.model.EVENT_OTHER_AUDIENCE.length != 0) {
        data.append(
          "EVENT_OTHER_AUDIENCE",
          JSON.stringify(this.model.EVENT_OTHER_AUDIENCE)
        );
      }
      data.append("CREATE_BY", 1);

      try {
        const response = await servicesModule0.createEvent(data);
      } catch (error) {
        console.log(error);
      }
    },

    async submit() {
      console.log("file record : " + fileRecords);
      this.compType.forEach(item => {
        if (item.selected == true) {
          this.selectedAudience.push(item.id);
        }
      });
      console.log(JSON.stringify(this.selectedAudience));
      const data = new FormData();
      data.append("title", this.title);
      data.append("content", this.content);
      data.append("compType", JSON.stringify(this.selectedAudience));
      data.append("startDate", moment(this.startDate).format("YYYY-MM-DD"));
      data.append("endDate", moment(this.endDate).format("YYYY-MM-DD"));

      try {
        const response = await servicesModule0.createEvent(data);
        this.selectedAudience = [];
      } catch (error) {
        console.log(error);
      }
    },
    back() {
      this.$router.go(-1);
    },

    deleteUploadedFile: function(fileRecord) {
      this.uploadFileOject = [];
      this.$refs.vueFileAgent.deleteUpload(
        this.uploadUrl,
        this.uploadHeaders,
        fileRecord
      );
      var i;
      for (i = 0; i < this.fileRecordsForUpload.length; i++) {
        this.uploadFileOject.push(this.fileRecordsForUpload[i].file);
      }
      console.log(this.uploadFileOject);
    },
    filesSelected: function(fileRecordsNewlySelected) {
      this.uploadFileOject = [];
      var validFileRecords = fileRecordsNewlySelected.filter(
        fileRecord => !fileRecord.error
      );
      this.fileRecordsForUpload = this.fileRecordsForUpload.concat(
        validFileRecords
      );
      //this.uploadFileName = this.fileRecordsForUpload[0].file.name;
      //  this.uploadFileOject = this.fileRecordsForUpload[0].file;
      // this.uploadFileOject.push(this.fileRecordsForUpload[0].file);
      var i;
      for (i = 0; i < this.fileRecordsForUpload.length; i++) {
        this.uploadFileOject.push(this.fileRecordsForUpload[i].file);
      }
      console.log(this.uploadFileOject);
    },

    onBeforeDelete: function(fileRecord) {
      var i = this.fileRecordsForUpload.indexOf(fileRecord);
      if (i !== -1) {
        console.log("before delete");
        // queued file, not yet uploaded. Just remove from the arrays
        this.fileRecordsForUpload.splice(i, 1);
        var k = this.fileRecords.indexOf(fileRecord);
        if (k !== -1) this.fileRecords.splice(k, 1);
        this.uploadFileOject = [];
        for (i = 0; i < this.fileRecordsForUpload.length; i++) {
          this.uploadFileOject.push(this.fileRecordsForUpload[i].file);
        }
        console.log(this.uploadFileOject);
      } else {
        this.$refs.vueFileAgent.deleteFileRecord(fileRecord);
        // if (confirm("Are you sure you want to delete?")) {
        //   this.$refs.vueFileAgent.deleteFileRecord(fileRecord); // will trigger 'delete' event
        // }
      }
    },
    fileDeleted: function(fileRecord) {
      console.log("file delete");
      var i = this.fileRecordsForUpload.indexOf(fileRecord);
      if (i !== -1) {
        this.fileRecordsForUpload.splice(i, 1);
      } else {
        this.deleteUploadedFile(fileRecord);
      }
    }
  },
  data() {
    return {
      selected: false,
      fileRecords: [],
      fileRecordsForUpload: [],
      uploadFileOject: [],
      startDate: "",
      endDate: "",
      content: "",
      title: "",
      content: "<h5></h5>",
      selectedAudience: [],

      model: {
        EVENT_TITLE: "",
        EVENT_CONTENT: "",
        EVENT_DATE_START: "",
        EVENT_DATE_END: "",
        EVENT_DISTRIBUTOR_AUDIENCE: [],
        EVENT_CONSULTANT_AUDIENCE: [],
        EVENT_OTHER_AUDIENCE: [],
        CREATE_BY: ""
      },

      compType: [],
      consultantType: [],
      otherType: []
    };
  }

  // watch: {
  //   compType(value) {
  //     console.log(value);
  //     // this.$emit('onchange', value);
  //     // or generate/simulate a native events (not sure how, but its outside Vue's realm I think
  //   }
  // }
};
</script>
<style lang="scss">
.bgcolor {
  background-color: white !important;
}
</style>
