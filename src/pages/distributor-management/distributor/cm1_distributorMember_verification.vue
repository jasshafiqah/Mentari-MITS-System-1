<template>
  <div class="content">
    <div class="container-fluid">
      <div class="card-body">
        <div class="row justify-content-center">
          <div class="col-lg-12">
            <va-card  class="strpied-tabled-with-hover"
              body-classes="table-full-width table-responsive">

              <h4 slot="header" class="card-title">User Registration Details </h4>

              <br>
              <div>
                <tabs
                  :tabs="tabs"
                  :currentTab="currentTab"
                  :wrapper-class="'default-tabs'"
                  :tab-class="'default-tabs__item'"
                  :tab-active-class="'default-tabs__item_active'"
                  :line-class="'default-tabs__active-line'"
                  @onClick="handleClick"
                />
                <div class="content">
                  <div v-if="currentTab === 'tab1'">
                    <div class="ml-4">
                      <br />
                      <table class="table table-striped">
                        <tbody>
                          <tr>
                            <td style="width:20%">Name</td>
                            <td style="width:2%">:</td>
                            <td>Noor Atikah Binti Ramlan</td>
                          </tr>
                          <tr>
                            <td>NRIC No</td>
                            <td>:</td>
                            <td>890408-10-3456</td>
                          </tr>
                          <tr>
                            <td>Passport No</td>
                            <td>:</td>
                            <td>-</td>
                          </tr>
                          <tr>
                            <td>Telephone Number</td>
                            <td>:</td>
                            <td>014-6218910</td>
                          </tr>
                          <tr>
                            <td>Email</td>
                            <td>:</td>
                            <td>atikah@affin.com</td>
                          </tr>
                          <tr>
                            <td>Address</td>
                            <td>:</td>
                            <td>3, Jalan Dato Abdul Aziz, Seksyen 14</td>
                          </tr>
                          <tr>
                            <td>Postcode</td>
                            <td>:</td>
                            <td>46100</td>
                          </tr>
                          <tr>
                            <td>City</td>
                            <td>:</td>
                            <td>Petaling Jaya,</td>
                          </tr>
                          <tr>
                            <td>State</td>
                            <td>:</td>
                            <td>Selangor</td>
                          </tr>

                        </tbody>
                      </table>

                    </div>
                  </div>
                  <div v-if="currentTab === 'tab2'">
                    <div class="ml-4">
                      <br />
                      <table class="table table-striped">
                        <tbody>
                          <tr>
                            <td style="width:20%">Division</td>
                            <td style="width:2%">:</td>
                            <td>Sales & Marketing</td>
                          </tr>
                          <tr>
                            <td>Department</td>
                            <td>:</td>
                            <td>Training</td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                  </div>
                  <div v-if="currentTab === 'tab3'">
                    <div class="ml-4">
                      <br />
                      <table class="table table-striped">
                        <tbody>
                          <tr>
                            <td style="width:20%">Passport Size Picture</td>
                            <td style="width:2%">:</td>
                            <td><a target="blank" href="https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf" class="row">Document 1.pdf</a>
                            </td>
                          </tr>
                          <tr>
                            <td>Supported Document</td>
                            <td>:</td>
                            <td><a target="blank" href="https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf" class="row">Document 1.pdf</a>
                            </td>
                          </tr>

                        </tbody>
                      </table>
                    </div>
                  </div>

                </div>
              </div>

            </va-card>
          </div>
        </div>
      </div>
    </div>
    <div class="container-fluid">
      <div class="card-body">
        <div class="row justify-content-center">
          <div class="col-lg-12">
            <card  class="strpied-tabled-with-hover" body-classes="table-full-width table-responsive">
              <h4 slot="header" class="card-title">Form Verification</h4>
              <form >
                <vue-form-generator
                  :model="model"
                  :schema="schema"
                  :options="formOptions"
                  @model-updated="onModelUpdated"
                  ref="statusForm" >
                </vue-form-generator>

                <button  type="submit" class=" ml-3 btn btn-primary btn-fill float-left btn-md">
                  <i class="fa fa-step-backward" /> &nbsp; Back
                </button>
                <div class="float-right">
                  <button @click="saveAsDraft" type="button" class="mr-2 btn btn-info btn-fill btn-md">
                    <i class="fa fa-pencil-square-o" /> &nbsp; Save as draft
                  </button>
                  <button  @click="submit"  type="button"  class="mr-2 btn btn-warning btn-fill btn-md">
                    <i class="fa fa-reply" /> &nbsp; Return
                  </button>
                  <button  @click="submit"  type="button"  class="btn btn-success btn-fill btn-md">
                    <i class="fa fa-paper-plane" /> &nbsp; Verified
                  </button>
                </div>
              </form>
            </card>
          </div>
        </div>
      </div>

    </div>
  </div>

</template>
<script>
import 'vue-form-generator/dist/vfg.css'
import Tabs from 'vue-tabs-with-active-line'

const TABS = [{
  title: 'User Profile',
  value: 'tab1',
},
{
  title: 'Division & Department',
  value: 'tab2',
},
{
  title: 'Uploaded Document',
  value: 'tab3',
},

]
export default {
  components: {
    Tabs,
  },

  data () {
    return {
      tabs: TABS,
      currentTab: 'tab1',

      // FORM
      schema: {
        fields: [

          {
            type: 'textArea',
            label: 'Remark',
            model: 'Remark',
            hint: 'Max 500 characters',
            max: 500,
            rows: 4,
            required: true,
            validator: 'string',
          },

        ],
      },

      formOptions: {
        validateAfterLoad: false,
        validateAfterChanged: true,
        validateAsync: true,
      },
    }
  },

  methods: {
    saveAsDraft () {},
    validatestatusForm () {
      return this.$refs.statusForm.validate()
    },
    submit: async function () {
      const response = await this.validatestatusForm()
      if (response) {
        //
      } else {

      }
    },
    handleClick (newTab) {
      this.currentTab = newTab
    }, // end tab
    onModelUpdated (newVal, schema) {
      console.log(schema)
      // eslint-disable-next-line eqeqeq
      if (schema == 'document') {
        console.log(newVal.replace('C:\\fakepath\\', ''))
        this.model.document = newVal.replace('C:\\fakepath\\', '')
      }
    },
  },
}

</script>

<style lang="scss" scoped>

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
    &_active {
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
      margin-left: 20px;
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
</style>
