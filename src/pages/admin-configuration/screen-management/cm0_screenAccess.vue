/* eslint-disable no-unused-expressions */
<template>
  <va-card>
    <!-- authorization -->
    <vudal name="editAuthorizationLevel" class="widthModal">
      <div class="header">
        Edit Edit Division<i class="close icon">&times;</i>
      </div>
      <div class="content">
        <vue-form-generator
          :model="model"
          :schema="editDivisionSchema"
          :options="formOptions"
        />
      </div>
      <div class="actions">
        <button
          type="button"
          class="cancel vudal-btn mr-2 btn btn-danger btn-fill btn-md"
        >
          <i class="fa fa-times" /> &nbsp; Close
        </button>
        <button
          @click="updateDivisionAPI"
          type="button"
          class="cancel vudal-btn mr-2 btn btn-primary btn-fill btn-md"
        >
          <i class="fa fa-edit" /> &nbsp; Update
        </button>
      </div>
    </vudal>
    <!-- division -->
    <vudal name="simpleModalEditDivision" class="widthModal">
      <div class="header">
        Edit Edit Division<i class="close icon">&times;</i>
      </div>
      <div class="content">
        <vue-form-generator
          :model="model"
          :schema="editDivisionSchema"
          :options="formOptions"
        />
      </div>
      <div class="actions">
        <button
          type="button"
          class="cancel vudal-btn mr-2 btn btn-danger btn-fill btn-md"
        >
          <i class="fa fa-times" /> &nbsp; Close
        </button>
        <button
          @click="updateDivisionAPI"
          type="button"
          class="cancel vudal-btn mr-2 btn btn-primary btn-fill btn-md"
        >
          <i class="fa fa-edit" /> &nbsp; Update
        </button>
      </div>
    </vudal>
    <!-- department -->
    <vudal name="simpleModalEditDepartment" class="widthModal">
      <div class="header">
        Screen Authorization Details<i class="close icon">&times;</i>
      </div>
      <div class="content">
        <vue-form-generator
          :model="model"
          :schema="editDepartmentSchema"
          :options="formOptions"
        />
      </div>
      <div class="actions">
        <button
          type="button"
          class="cancel vudal-btn mr-2 btn btn-danger btn-fill btn-md"
        >
          <i class="fa fa-times" /> &nbsp; Close
        </button>
        <button
          @click="updateDepartmentAPI"
          type="button"
          class="cancel vudal-btn mr-2 btn btn-primary btn-fill btn-md"
        >
          <i class="fa fa-edit" /> &nbsp; Update
        </button>
      </div>
    </vudal>
    <!-- group -->

    <h4 slot="header" class="card-title">Screen Access</h4>
    <div class="">
      <br />
      <tabs
        :tabs="tabs"
        :currentTab="currentTab"
        :wrapper-class="'default-tabs'"
        :tab-class="'default-tabs__item'"
        :tab-active-class="'default-tabs__item_active'"
        :line-class="'default-tabs__active-line'"
        @onClick="handleClick"
      />

      <div v-if="currentTab === 'tab1'">
        <br />
        <vue-form-generator
          :model="model"
          :schema="Tab1Schema"
          :options="formOptions"
          @model-updated="onModuleUpdated"
          ref="Tab1Form"
        >
        </vue-form-generator>
        <br />
        <!-- list -->
        <va-card class="mt-2">
          <va-data-table
            :datacount="screenListCount"
            :dataperpage="parseInt(perPage)"
            :fields="moduleFields"
            :data="moduleFilteredData"
            no-data-label="No data found"
            :per-page="parseInt(perPage)"
            clickable
          >
            <template slot="no" slot-scope="row">
              {{ row.rowIndex + 1 }}
            </template>

            <template slot="actions" slot-scope="props">
              <va-checkbox class="col-2" v-model="props.rowData.selected" />
            </template>
          </va-data-table>
        </va-card>
        <br />
        <vue-form-generator
          :model="model"
          :schema="Tab1ConfigSchema"
          :options="formOptions"
          @model-updated="onConfigUpdated"
          ref="Tab1Form"
        >
        </vue-form-generator>
        <br /><br />
        <button
          @click="back"
          type="button"
          class="ml-2 btn btn-primary btn-fill float-left btn-md"
        >
          Back
        </button>
        <button
          type="submit"
          @click.prevent="submit"
          class="ml-2 btn btn-primary btn-fill float-right btn-md"
        >
          &nbsp; Submit
        </button>
      </div>
      <div v-if="currentTab === 'tab2'">
        <br />
        <vue-form-generator
          :model="model"
          :schema="Tab2Schema"
          :options="formOptions"
          @model-updated="onModuleUpdated"
          ref="Tab2Form"
        >
        </vue-form-generator>
        <va-card class="mt-2">
          <va-data-table
            :datacount="screenAccessListCount"
            :dataperpage="parseInt(perPage)"
            :fields="groupAuthorizationSchema"
            :data="groupAuthorizationFilteredData"
            :per-page="parseInt(perPage)"
            clickable
          >
            <template slot="no" slot-scope="row">
              {{ row.rowIndex + 1 }}
            </template>

            <template slot="actions" slot-scope="props">
              <h5 class="mt-1">
                <span
                  v-b-tooltip.hover
                  title="Details"
                  v-on:click="showScreenDetails(props.rowData)"
                  class="badge badge-primary mr-2"
                >
                  <i class="fa fa-info-circle"></i
                ></span>
                <span
                  v-b-tooltip.hover
                  title="Edit"
                  v-on:click="showAuthorizationLevelDetails(props.rowData)"
                  class="badge badge-primary mr-2"
                >
                  <i class="fa fa-edit"></i
                ></span>
              </h5>
            </template>
          </va-data-table>
        </va-card>
      </div>
    </div>
  </va-card>
</template>
<script>
// eslint-disable-next-line no-unused-vars
import Vudal from "vudal";
import "vue-form-generator/dist/vfg.css";
import Tabs from "vue-tabs-with-active-line";
import * as servicesModule04 from "../../../app/module0/services04";
import * as servicesModule0 from "../../../app/module0/services";
import * as servicesModule1 from "../../../app/module1/services";

const TABS = [
  { title: "Setting 1 : Access Screen", value: "tab1" },
  { title: "Setting 2 : Authorization Level Screen", value: "tab2" }
];

export default {
  mounted() {
    this.getAllScreens();
    this.getAllModule();
    this.getDivision();
    this.getScreenAccesses();
    this.getAuthorizationLevel();
    // this.getUser();
    this.getAllSubmodule();
  },
  components: {
    Tabs,
    Vudal
  },
  methods: {
    async submit() {
      this.checkboxArray = [];
      this.screenList.forEach(item => {
        if (item.selected == true) {
          this.checkboxArray.push(item.MANAGE_SCREEN_ID);
        }
      });
      // alert(this.checkboxArray)
      const data = new FormData();
      data.append("MANAGE_GROUP_ID", this.model.groupSelection.MANAGE_GROUP_ID);
      data.append("AUTHORIZATION_LEVEL_ID", this.authorizationLevel);
      data.append("MANAGE_SCREEN_ID", JSON.stringify(this.checkboxArray));
      try {
        const response = await servicesModule0.createScreenAccess(data);
      } catch (error) {
        console.log(error);
      }
    },
    checkBox(screenId, id) {
      if (document.getElementById("checkboxId_" + id).checked) {
        this.checkboxArray.push(screenId);
      } else {
        const indexToRemove = this.checkboxArray.indexOf(screenId);
        if (indexToRemove > -1) {
          this.checkboxArray.splice(indexToRemove, 1);
        }
      }
    },

    getScreenAccesses: async function() {
      const response = await servicesModule04.getScreenAccesses();
      this.screenAccessList = response;
      this.screenAccessListCount = this.screenAccessList.length;
    },

    getDivision: async function() {
      const response = await servicesModule04.getAllDivisionList();
      this.divisionList = response;

      // alert(this.divisionList)
    },

    getAuthorizationLevel: async function() {
      const response = await servicesModule04.getAuthorizationLevel(
        "AUTHORIZATIONLEVEL"
      );
      this.authorizationList = response;
    },

    onModuleUpdated: async function(newVal, schema) {
      if (schema == "moduleSelection" && newVal) {
        this.model.submoduleSelection = "";

        console.log(newVal.MANAGE_MODULE_ID);
        const response = await servicesModule04.getSubmodule(
          newVal.MANAGE_MODULE_ID
        );
        this.submoduleList = response;
        console.log("data sini : " + this.submoduleList);

        this.getScreen();
      }

      if (schema == "submoduleSelection" && newVal) {
        this.getScreen();
      }

      if (schema == "divisionSelection" && newVal) {
        this.model.departmentSelection = "";

        const response = await servicesModule04.getDepartmentByDivisionId(
          newVal.MANAGE_DIVISION_ID
        );
        this.departmentList = response;
        console.log("data sini : " + this.departmentList);

        this.getScreen();
      }
    },
    onConfigUpdated: async function(newVal, schema) {
      if (schema == "divisionSelection" && newVal) {
        this.model.departmentSelection = "";
        this.model.groupSelection = "";
        this.model.authorizationSelection = "";

        const response = await servicesModule04.getDepartmentByDivisionId(
          newVal.MANAGE_DIVISION_ID
        );
        this.departmentList = response;
        this.getDepartment();
      } else if (schema == "departmentSelection" && newVal) {
        this.model.groupSelection = "";
        this.model.authorizationSelection = "";

        const response = await servicesModule04.getGroupByDepartmentId(
          newVal.MANAGE_DEPARTMENT_ID
        );
        this.groupList = response;
        this.getGroup();
      } else if (schema == "groupSelection" && newVal) {
        this.model.authorizationSelection = "";

        const response = await servicesModule04.getDepartmentByDivisionId(
          newVal.MANAGE_DEPARTMENT_ID
        );
        // this.departmentList = response;
      } else if (schema == "authorizationSelection" && newVal) {
        this.authorizationLevel = newVal.AUTHORIZATION_LEVEL_ID;
      }
    },
    handleClick(newTab) {
      this.currentTab = newTab;
    }, // end tab
    back() {
      this.$router.go(-1);
    }, // back button

    //--------------------module---------------------------/
    getAllModule: async function() {
      const response = await servicesModule04.getAllModule();
      this.moduleList = response;
    },

    getScreen: async function() {
      const response = await servicesModule04.getScreen(
        this.model.moduleSelection.MANAGE_MODULE_ID,
        this.model.submoduleSelection.MANAGE_SUBMODULE_ID
      );
      this.screenList = response;
      this.screenListCount = this.screenList.length;
    },

    getAllScreens: async function() {
      const response = await servicesModule04.getAllScreens();
      this.screenList = response;
      this.screenListCount = this.screenList.length;
    },

    getDepartment: async function() {
      const response = await servicesModule04.getDepartmentByDivisionId(
        this.model.divisionSelection.MANAGE_DIVISION_ID
      );
      this.departmentList = response;
    },
    getGroup: async function() {
      const response = await servicesModule04.getGroupByDepartmentId(
        this.model.departmentSelection.MANAGE_DEPARTMENT_ID
      );
      this.groupList = response;
    },

    getAllSubmodule: async function() {
      if (this.hasModule) {
        // const response = await servicesModule0.getSubmodule(
        //   this.model.moduleSelection.MANAGE_MODULE_ID
        // );
        // this.submoduleList = response;
      }
    },
    async updateDivisionAPI() {
      try {
        const data = new FormData();
        data.append("MANAGE_MODULE_ID", this.MANAGE_MODULE_ID);
        data.append("MOD_NAME", this.model.editDivisionName);

        // console.log("data :" + data);
        // eslint-disable-next-line no-unused-vars
        const response = await servicesModule0.updateDivision(data);
        this.getAllModule();
      } catch (error) {
        console.log(error);
      }
    },
    //----------------------department-----------------/
    // getAllDepartment: async function() {
    //   const response = await servicesModule0.getAllDepartmentList();
    //   this.departmentList = response;
    // },
    async addDepartment() {
      if (this.$refs.Tab2Form.validate()) {
        const data = new FormData();
        data.append(
          "MANAGE_MODULE_ID",
          this.model.divisionSelection.MANAGE_MODULE_ID
        );
        data.append("DPMT_NAME", this.model.departmentName);
        try {
          // eslint-disable-next-line no-unused-vars
          const response = await servicesModule0.createDepartment(data);
          this.getDepartment();
        } catch (error) {
          console.log(error);
        }
      }
    },
    async deleteDepartment(data) {
      // eslint-disable-next-line no-unused-vars
      const response = await servicesModule0.deleteDepartment(
        data.MANAGE_DEPARTMENT_ID
      );
      this.getDepartment();
    },
    async showAuthorizationLevelDetails(data) {
      this.$modals.editAuthorizationLevel.$show();
    },
    async showScreenDetails(data) {
      // eslint-disable-next-line no-unused-vars
      const response = await servicesModule0.getDepartmentById(
        data.MANAGE_DEPARTMENT_ID
      );
      this.$modals.simpleModalEditDepartment.$show();
      this.MANAGE_DEPARTMENT_ID = response.MANAGE_DEPARTMENT_ID;
      this.model.DIVISION = response;
      this.model.DPMT_NAME = response.DPMT_NAME;
    },
    async updateDepartmentAPI() {
      try {
        const data = new FormData();
        data.append("MANAGE_DEPARTMENT_ID", this.MANAGE_DEPARTMENT_ID);
        data.append(
          "MANAGE_MODULE_ID",
          this.model.moduleSelection.MANAGE_MODULE_ID
        );
        data.append("DPMT_NAME", this.model.editDepartmentName);

        // console.log("data :" + data);
        // eslint-disable-next-line no-unused-vars
        const response = await servicesModule0.updateDepartment(data);
        this.getDepartment();
      } catch (error) {
        console.log(error);
      }
    }
    //----------------------group-----------------/
  },

  computed: {
    //tab1
    moduleFields() {
      return [
        {
          name: "__slot:no",
          title: "No"
        },
        {
          name: "MOD_NAME",
          title: "Module Name"
        },
        {
          name: "SUBMOD_NAME",
          title: "Submodule Name"
        },
        {
          name: "SCREEN_NAME",
          title: "Screen Name"
        },
        {
          name: "SCREEN_DESCRIPTION",
          title: "Description"
        },
        {
          name: "__checkbox",
          titleClass: "center aligned",
          dataClass: "center aligned"
        }
      ];
    },
    moduleFilteredData() {
      if (!this.term || this.term.length < 1) {
        return this.screenList;
      }
    },
    //tab2
    groupAuthorizationSchema() {
      return [
        {
          name: "__slot:no",
          title: "No"
        },
        {
          name: "GROUP_NAME",
          title: "Group"
        },
        {
          name: "SET_PARAM",
          title: "Authorization Level"
        },
        {
          name: "__slot:actions",
          title: "Action",
          dataClass: "justify-content-center",
          width: "15%"
        }
      ];
    },
    groupAuthorizationFilteredData() {
      if (!this.term || this.term.length < 1) {
        return this.screenAccessList;
      }
    },
    //tab3
    groupFields() {
      return [
        {
          name: "__slot:no",
          title: "No"
        },
        {
          name: "MOD_NAME",
          title: "Module Name"
        },
        {
          name: "DPMT_NAME",
          title: "Department Name"
        },
        {
          name: "GROUP_NAME",
          title: "Group Name"
        },
        {
          name: "__slot:actions",
          title: "Action",
          dataClass: "justify-content-center",
          width: "15%"
        }
      ];
    },
    groupFilteredData() {
      if (!this.term || this.term.length < 1) {
        return this.groupList;
      }
    }
  },

  data() {
    return {
      screenListCount: null,
      screenAccessListCount: null,
      checkboxArray: [],
      authorizationLevel: null,
      screenListCurrentCount: null,
      header: "",
      field: [],
      count: 1,
      term: null,
      hasModule: false,
      perPage: "10",
      perPageOptions: ["4", "6", "10", "20"],
      moduleList: [],
      screenList: [],
      submoduleList: [],
      screenAccessList: [],
      divisionList: [],
      authorizationList: [],
      staffList: [],
      departmentList: [],
      groupList: [],
      currentPage: 1,

      tabs: TABS,
      currentTab: "tab1",

      model: {
        divisionName: "",
        editDivisionName: "",

        moduleSelection: "",
        submoduleSelection: "",
        departmentSelection: "",
        groupSelection: "",
        authorizationSelection: "",
        departmentName: "",
        editDepartmentName: "",

        DIVISION: "",
        DPMT_NAME: ""
      },
      //Module
      editDivisionSchema: {
        groups: [
          {
            styleClasses: "row",
            fields: [
              {
                type: "input",
                inputType: "Text",
                model: "editDivisionName",
                label: "Module Name",
                placeholder: "Enter division",
                required: true,
                styleClasses: "col-md-12"
              }
            ]
          }
        ]
      },
      //Module & submodule
      Tab1Schema: {
        groups: [
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "moduleSelection",
                placeholder: "Select Module",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_MODULE_ID",
                  label: "MOD_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.moduleList;
                },
                styleClasses: "col-md-6"
              },
              {
                type: "vueMultiSelect",
                model: "submoduleSelection",
                placeholder: "Select Submodule",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_SUBMODULE_ID",
                  label: "SUBMOD_NAME",
                  searchable: true
                },
                disabled: (model, schema) => {
                  if (this.model.moduleSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                values: (model, schema) => {
                  return this.submoduleList;
                },
                styleClasses: "col-md-6"
              }
            ]
          }
        ]
      },
      Tab2Schema: {
        groups: [
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "divisionSelection",
                placeholder: "Select Division",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_DIVISION_ID",
                  label: "DIV_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.divisionList;
                },
                styleClasses: "col-md-3"
              },
              {
                type: "vueMultiSelect",
                model: "departmentSelection",
                placeholder: "Select Department",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_DEPARTMENT_ID",
                  label: "DPMT_NAME",
                  searchable: true
                },
                disabled: (model, schema) => {
                  if (this.model.divisionSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                values: (model, schema) => {
                  return this.departmentList;
                },
                styleClasses: "col-md-3"
              },
              {
                type: "vueMultiSelect",
                model: "groupTab2Selection",
                placeholder: "Select Group",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_GROUP_ID",
                  label: "GROUP_NAME",
                  searchable: true
                },
                disabled: (model, schema) => {
                  if (this.model.moduleSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                values: (model, schema) => {
                  return this.groupList;
                },
                styleClasses: "col-md-3"
              },
              {
                type: "vfg-search",
                model: "searchTab2Selection",
                placeholder: "Search",
                styleClasses: "col-md-3"
              }
            ]
          }
        ]
      },
      Tab1ConfigSchema: {
        groups: [
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "divisionSelection",
                label: "Division Name",
                required: true,
                placeholder: "Select Division",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_DIVISION_ID",
                  label: "DIV_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.divisionList;
                },
                styleClasses: "col-md-6"
              },
              {
                type: "vueMultiSelect",
                model: "departmentSelection",
                label: "Department Name",
                disabled: (model, schema) => {
                  if (this.model.divisionSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                required: true,
                placeholder: "Select Department",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_DEPARTMENT_ID",
                  label: "DPMT_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.departmentList;
                },
                styleClasses: "col-md-6"
              }
            ]
          },
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "groupSelection",
                label: "Group Name",
                required: true,
                disabled: (model, schema) => {
                  if (this.model.departmentSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                placeholder: "Select Group",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_GROUP_ID",
                  label: "GROUP_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.groupList;
                },
                styleClasses: "col-md-6"
              },
              {
                type: "vueMultiSelect",
                model: "authorizationSelection",
                label: "Authorization Level",
                required: true,
                placeholder: "Select Authorization Level",
                selectOptions: {
                  multiple: false,
                  key: "AUTHORIZATION_LEVEL_ID",
                  label: "AUTHORIZATION_LEVEL_NAME",
                  searchable: true
                },
                disabled: (model, schema) => {
                  if (this.model.groupSelection) {
                    return false;
                  } else {
                    return true;
                  }
                },
                values: (model, schema) => {
                  return this.authorizationList;
                },
                styleClasses: "col-md-6"
              }
            ]
          },
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "staffSelection",
                label: "Staff Name",
                placeholder: "Select Staff",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_STAFF_ID",
                  label: "STAFF_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.staffList;
                },
                styleClasses: "col-md-12"
              }
            ]
          }
        ]
      },
      editDepartmentSchema: {
        groups: [
          {
            styleClasses: "row mt-2",
            fields: [
              {
                type: "vueMultiSelect",
                model: "DIVISION",
                label: "Module Name",
                placeholder: "Select Division",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_MODULE_ID",
                  label: "MOD_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.moduleList;
                },
                styleClasses: "col-md-12",
                required: true,
                validator: "required"
              },
              {
                type: "input",
                inputType: "Text",
                model: "DPMT_NAME",
                label: "Department Name",
                placeholder: "Enter department",
                required: true,
                styleClasses: "col-md-12",
                validator: "required"
              }
            ]
          }
        ]
      },

      dpmtFilterSchema: {
        groups: [
          {
            styleClasses: "row",
            fields: [
              {
                type: "vueMultiSelect",
                model: "editDivisionSelection",
                label: "Module Name",
                placeholder: "Select Division",
                selectOptions: {
                  multiple: false,
                  key: "MANAGE_MODULE_ID",
                  label: "MOD_NAME",
                  searchable: true
                },
                values: (model, schema) => {
                  return this.divisionList;
                },
                styleClasses: "col-md-6",
                required: true,
                validator: "required"
              }
            ]
          }
        ]
      },
      //Group

      formOptions: {
        validateAfterChanged: true
      }
    };
  }
};
</script>
<style lang="scss">
.va-data-table__pagination {
  margin-top: 1rem;
  display: -webkit-box;
  display: -ms-flexbox;
  display: inherit;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: flex-end;
}

.default-tabs {
  position: relative;
  margin: 0 auto;

  &__item {
    display: inline-block;
    margin: 0 5px;
    padding: 10px;
    padding-bottom: 8px;
    font-size: 16px;
    letter-spacing: 0.8px;
    color: gray;
    text-decoration: none;
    border: none;
    background-color: transparent;
    border-bottom: 2px solid transparent;
    cursor: pointer;
    transition: all 0.25s;

    &__active {
      color: black;
    }

    &:hover {
      border-bottom: 2px solid gray;
      color: black;
    }

    &:focus {
      outline: none;
      border-bottom: 2px solid gray;
      color: black;
    }

    &:first-child {
      margin-left: 0;
    }

    &:last-child {
      margin-right: 0;
    }
  }

  &__active-line {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    background-color: black;
    transition: transform 0.4s ease, width 0.4s ease;
  }
}

.content {
  margin-top: 30px;
  font-size: 20px;
}

.inline {
  display: inline-flex;
  float: right;
}
</style>
