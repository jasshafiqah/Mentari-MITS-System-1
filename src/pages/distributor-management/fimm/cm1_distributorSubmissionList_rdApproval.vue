<template>
  <va-card>
    <template slot="header">
      <br />
      <div class="row">
        <div class="col">
          <h4 class="card-title">List of New Distributor Application</h4>
        </div>
        <div class="col float-right">
          <div class="float-right">
            <h5>
              <b-badge
                v-b-tooltip.hover
                title="search"
                v-b-toggle.collapse-1
                class="mr-1"
                variant="secondary"
              >
                <i class="fa fa-search"></i>
              </b-badge>
            </h5>
          </div>
        </div>
      </div>
    </template>
    <b-collapse id="collapse-1" class="mt-2">
      <b-card>
        <vue-form-generator :model="model" :schema="schema" ref="firstTabForm">
        </vue-form-generator>
        <button type="button" class="ml-2 btn btn-sm btn-primary">
          Search
        </button>
        <button
          @click.prevent="reset"
          type="button"
          class="ml-2 btn btn-sm btn-danger"
        >
          Reset
        </button>
      </b-card>
    </b-collapse>
    <br />
    <div class="alignleft">
      <div class="flex xs12 md3 left">
        <va-input
          :value="term"
          :placeholder="$t('Search')"
          @input="search"
          removable
        >
          <va-icon name="fa fa-search" slot="prepend" />
        </va-input>
      </div>
    </div>
    <br />
    <!-- Main table element -->
    <!-- <div class="col-ml-2"> -->
    <va-data-table
      :fields="fields"
      :data="filteredData"
      :per-page="parseInt(perPage)"
    >
      <!-- A virtual column -->
      <template slot="no" slot-scope="row">
        {{ row.rowIndex + 1 }}
      </template>
      <!-- <template slot="ANNOUNCEMENT_STATUS" slot-scope="props">
        <div v-if="props.rowData.ANNOUNCEMENT_STATUS == 1">
          Draft
        </div>
        <div v-if="props.rowData.ANNOUNCEMENT_STATUS == 2">
          Publish
        </div>
      </template> -->
      <template slot="actions" slot-scope="props">
        <h4 class="mt-2">
          <span
            v-b-tooltip.hover
            title="detail"
            v-on:click="details(props.rowData)"
            class="badge badge-success mr-2"
          >
            <i class="fa fa-eye"></i
          ></span>
        </h4>
      </template>
    </va-data-table>
  </va-card>
</template>
<script>
import Vue from "vue";
import VueFormGenerator from "vue-form-generator";
import "vue-form-generator/dist/vfg.css";
import { debounce } from "lodash";
import * as servicesModule0 from "../../../app/module0/services";
import * as services06Module0 from "../../../app/module0/services06";

Vue.use(VueFormGenerator);

export default {
  props: {},
  mounted() {
    this.getDistributorApplicationList();
  },
  methods: {
    search: debounce(function(term) {
      this.term = term;
    }, 400),
    async getDistributorApplicationList() {
      const user = localStorage.getItem("user");
      const response = await services06Module0.getDistributorApplicationList(
        JSON.parse(user).USER_GROUP_ID
      );
      this.distributorList = response;
    },

    search: debounce(function(term) {
      this.term = term;
    }, 400),

    details(data) {
      alert(data.DISTRIBUTOR_ID);
      this.$router.push({
        name: "distributor-SubmissionDetails-rdApproval",
        params: { dist_id: data.DISTRIBUTOR_ID }
      });
    },

    reset() {}
  },

  computed: {
    fields() {
      return [
        {
          name: "__slot:marker",
          width: "30px",
          dataClass: "text-center"
        },
        {
          name: "__slot:no",
          title: "No"
        },
        {
          name: "DIST_NAME",
          title: "Distributor Name",
          width: "30%"
        },
        {
          name: "DIST_REGI_NUM1",
          title: "Reg. Number"
          // width: "40%"
        },
        {
          name: "DIST_REGI_NUM2",
          title: "New Reg. Number"
          // width: "40%"
        },
        {
          name: "DIST_PHONE_NUMBER",
          title: "Phone Number"
          // width: "40%"
        },
        {
          name: "DIST_EMAIL",
          title: "Email"
          // width: "40%"
        },
        {
          name: "TS_PARAM",
          title: "Status"
          // width: "40%"
        },
        {
          name: "__slot:actions",
          title: "Action"
          //   dataClass: "justify-content-center",
          //   width: "15%"
        }
      ];
    },
    filteredData() {
      if (!this.term || this.term.length < 1) {
        return this.distributorList;
      }

      return this.distributorList.filter(item => {
        return this.term
          .toLowerCase()
          .split(" ")
          .every(
            v =>
              item.DIST_NAME.toLowerCase().includes(v) ||
              item.DIST_REGI_NUM1.toLowerCase().includes(v) ||
              item.DIST_REGI_NUM2.toLowerCase().includes(v) ||
              item.DIST_PHONE_NUMBER.toLowerCase().includes(v) ||
              item.DIST_EMAIL.toLowerCase().includes(v) ||
              item.TS_PARAM.toLowerCase().includes(v)
          );
      });
    }
  },
  data() {
    return {
      term: null,
      perPage: "6",
      perPageOptions: ["4", "6", "10", "20"],
      distributorList: [],
      model: {
        title: "",
        dateCreated: "",
        createdBy: "",
        status: ""
      },

      schema: {
        groups: [
          {
            styleClasses: "row",
            fields: [
              {
                type: "input",
                inputType: "text",
                model: "title",
                label: "Title",
                styleClasses: "col-md-6"
              },
              {
                type: "input",
                inputType: "date",
                label: "Date Created",
                model: "dateCreated",
                format: "YYYY-MM-DD HH:mm:ss",
                styleClasses: "col-md-6"
              },
              {
                type: "input",
                inputType: "text",
                model: "createdBy",
                label: "Created By",
                styleClasses: "col-md-6"
              },
              {
                type: "select",
                label: "Status",
                model: "status",
                styleClasses: "col-md-6",

                values: (model, schema) => {
                  return this.status;
                }
              }
            ]
          }
        ]
      },

      type: [
        {
          id: "UTMC",
          name: "UTMC"
        },
        {
          id: "IUTA",
          name: "IUTA"
        },
        {
          id: "CUTA",
          name: "CUTA"
        }
      ],
      status: [
        {
          id: "Publish",
          name: "Publish"
        },
        {
          id: "Draft",
          name: "Draft"
        }
      ],
      category: [
        {
          id: "Agent",
          name: "Agent"
        },
        {
          id: "Employee",
          name: "Employee"
        }
      ]
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

.alignleft {
  display: flex;
  justify-content: flex-end;
}
</style>
