<template>
  <va-card>
    <div class="row">
      <h4 slot="header" class="card-title">List Of Fund Submission (IUTA/IPRA/CUTA/CPRA)</h4>
      <br />
      <div class="col float-right">
        <div class="float-right">
          <span v-b-tooltip.hover
            title="Advanced Search">
            <h5>
              <b-badge v-b-toggle.collapse-1 class="mr-1" variant="secondary"
              ><i class="fa fa-search"></i
              ></b-badge>
            </h5>
          </span>
        </div>
      </div>
    </div>
    <!-- advanced search form -->
    <b-collapse id="collapse-1" class="mt-2">
      <b-card>
        <vue-form-generator
          :model="model"
          :schema="advancedSearchSchema"
          ref="advancedSearchForm"
        >
        </vue-form-generator>
        <br />
        <button
          @click="filterFundProfile"
          type="button"
          class="ml-2 btn btn-sm btn-primary"
        >
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
    <div class="float-right">
      <va-input
        :value="term"
        :placeholder="$t('Search')"
        @input="search"
        removable
      >
        <va-icon name="fa fa-search" slot="prepend" />
      </va-input>
    </div>

    <!-- list table-->
    <va-card class="mt-5 col-12">
      <va-data-table
        :fields="fundProfile"
        :data="fundProfileFilteredData"
        no-data-label="No data found"
        :per-page="parseInt(perPage)"
        :datacount="fundprofileRecordCount"
        :dataperpage="parseInt(perPage)"
        clickable
      >
        <template slot="no" slot-scope="row">
          {{ row.rowIndex + 1 }}
        </template>
        <template slot="actions" slot-scope="props">
          <h5 class="mt-1">
            <span
              v-b-tooltip.hover
              title="edit"
              v-on:click="viewRecord(props.rowData)"
              class="badge badge-primary mr-2"
            >
              <i class="fa fa-eye"></i
              ></span>
          </h5>
        </template>
      </va-data-table>

      <br />
      <br />
      <br />
      <button
        @click="back"
        type="button"
        class="btn btn-primary btn-fill float-left btn-md"
      >
        <i class="fa fa-step-backward" /> &nbsp; Back
      </button>
    </va-card>
  </va-card>
</template>

<script>
import 'vue-form-generator/dist/vfg.css'
import { debounce } from 'lodash'
import * as servicesModule5 from '../../../app/module5/services03'

export default {
  mounted () {
    this.getAllFundProfile()
    console.log(JSON.stringify(this))
  },
  components: {},

  methods: {
    // searh
    search: debounce(function (term) {
      this.term = term
      console.log(this.term)
    }, 400),

    // // advanceSearch
    // getAllFundProfile: async function () {
    //   const response = await servicesModule5.getAllClassification()
    //   this.classificationList = response
    // },
    async filterFundProfile () {
      try {
        const response = await servicesModule5.fundprofileRecordList(
          this.model.modelFilter,
        )
        if (response !== 'error') {
          this.fundprofileRecordList = response
        }
      } catch (error) {}
    },

    handleClick (newTab) {
      this.currentTab = newTab
    }, // end tab
    back () {
      this.$router.go(-1)
    }, // back button

    // getAllRecord
    getAllFundProfile: async function () {
      const response = await servicesModule5.getAllFundProfile()
      this.fundprofileRecordList = response
      this.fundprofileRecordCount = this.fundprofileRecordList.length
    },
    async viewRecord (data) {
      this.$router.push({
        name: 'fundCreation-review',
        params: { FUNDPROFILE: data },
      })
    },
    // async viewRecord (data) {
    //   this.$router.push({
    //     name: 'fundcreation-review',
    //     // params: { FUNDPROFILE: data },
    //   })
    // },
    // async addRecord () {},
  },

  computed: {
    fundProfile () {
      return [
        {
          name: '__slot:no',
          title: 'NO',
        },
        {
          name: 'CREATE_TIMESTAMP',
          title: 'SUBMISSION DATE',
          sortField: 'CREATE_TIMESTAMP',
        },
        {
          name: 'FIMM_FUND_CODE',
          title: 'FIMM FUND CODE',
          sortField: 'FIMM_FUND_CODE',
        },
        {
          name: 'FUND_CODE_MEMBER',
          title: 'MEMBER FUND CODE',
          sortField: 'FUND_CODE_MEMBER',
        },
        {
          name: 'FUND_NAME',
          title: 'FUND NAME',
          sortField: 'FUND_NAME',
        },
        {
          name: 'UMBRELLA_FUND_NAME',
          title: 'UMBRELLA FUND NAME',
          sortField: 'UMBRELLA_FUND_NAME',
        },
        {
          name: 'STATUS',
          title: 'STATUS',
          sortField: 'STATUS',
        },
        {
          name: '__slot:actions',
          title: 'ACTIONS',
          dataClass: 'justify-content-center',
          width: '8%',
        },
      ]
    },
    fundProfileFilteredData () {
      if (!this.term || this.term.length < 1) {
        return this.fundprofileRecordList
      }
      return this.fundprofileRecordList.filter(item => {
        // return item.title.toLowerCase().startsWith(this.term.toLowerCase()) ,
        return this.term

          .split(' ')
          .every(
            (v) =>
              item.FIMM_FUND_CODE.includes(v) ||
              item.FUND_CODE_MEMBER.includes(v) ||
              item.FUND_NAME.includes(v) ||
              item.UMBRELLA_FUND_NAME.includes(v),
          )
      })
    },

    // // eslint-disable-next-line vue/return-in-computed-property
    // caRecordFilteredData () {
    //   if (!this.term || this.term.length < 1) {
    //     return this.caRecordList
    //   }

    //   return this.caRecordList.filter((item) => {
    //     return this.term

    //       .split(' ')
    //       .every(
    //         (v) =>
    //           item.CONSULTANT_NAME.includes(v) ||
    //           item.CONSULTANT_NRIC.includes(v) ||
    //           item.CONSULTANT_FIMM_NO.includes(v) ||
    //           item.CONSULTANT_PASSPORT_NO.includes(v),
    //       )
    //   })
    // },
  },

  data () {
    return {
      header: '',
      field: [],
      term: null,
      perPage: '10',
      perPageOptions: ['50', '100', '150', '200'],
      currentPage: 1,
      // classificationList: [],
      fundprofileRecordList: [],
      fundprofileRecordCount: null,
      count: 1,

      model: {
        modelFilter: {
          FIMM_FUND_CODE: '',
          FUND_CODE_MEMBER: '',
          FUND_NAME: '',
          UMBRELLA_FUND_NAME: '',
        },
      },

      advancedSearchSchema: {
        groups: [
          {
            styleClasses: 'row',
            fields: [
              {
                type: 'input',
                inputType: 'text',
                model: 'modelFilter.FIMM_FUND_CODE',
                label: 'FIMM Fund Code',
                styleClasses: 'col-md-6',
              },
              {
                type: 'input',
                inputType: 'text',
                model: 'modelFilter.FUND_CODE_MEMBER',
                label: 'Member Fund Code',
                styleClasses: 'col-md-6',
              },
              {
                type: 'input',
                inputType: 'text',
                model: 'modelFilter.FUND_NAME',
                label: 'Fund Name',
                styleClasses: 'col-md-6',
              },
              {
                type: 'input',
                inputType: 'text',
                model: 'modelFilter.UMBRELLA_FUND_NAME',
                label: 'Umbrella Fund Name',
                styleClasses: 'col-md-6',
              },

              // {
              //   type: "select",
              //   model: "classification",
              //   label: "Classification",
              //   placeholder: "Select Classification",
              //   selectOptions: {
              //     multiple: false,
              //     value: "SETTING_GENERAL_ID",
              //     name: "SET_PARAM",
              //     searchable: true,
              //   },
              //   values: (model, schema) => {
              //     return this.classificationList;
              //   },
              //   styleClasses: "col-md-6",
              // },
            ],
          },
        ],
      },

      formOptions: {
        validateAfterChanged: true,
      },
    }
  },
}
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

.content {
  margin-top: 30px;
  font-size: 20px;
}
.inline {
  display: inline-flex;
  float: right;
}
</style>
