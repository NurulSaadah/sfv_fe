<template>
  <div id="layoutSidenav">
    <CommonSidebar />
    <div id="layoutSidenav_content">
      <CommonHeader />
      <main>
        <div class="container-fluid px-4">
          <div class="page-title">
            <h1>Pengurusan Pengguna</h1>
            <!-- <a href="#"><i class="fal fa-plus"></i> Add</a> -->
          </div>
          <div class="row">
            <div class="card mb-4">
              <div class="card-body">
                <nav>
                <ul id="nav-tab" role="tablist" class="nav sub-tab">
                  <li class="nav-item">
                    <a data-bs-toggle="tab" href="#nav-home1" role="tab" aria-controls="nav-home" aria-selected="true" class="nav-link active">
                     Daftar Pengguna Baru</a>
                  </li>
                  </ul>
                  </nav>
                  <div id="nav-tabContent" class="tab-content"></div>
                   <div id="nav-home1" role="tabpanel" class="tab-pane fade show active">
                    <div class="content-subtab">
                  <form
                    class="g-3 mt-3"
                    method="post"
                    @submit.prevent="onCreateStaff"
                  >
                    <div class="row">
                      <div class="col-md-6 mb-4">
                        <label for="" class="form-label">Nama Penuh Pemohon<span style="color:red">*</span></label>
                        <input
                          type="text"
                          class="form-control"
                          placeholder="Nama Penuh"
                          v-model="name"
                          v-on:keypress="isLetter($event)"
                        />
                      </div>

                      <div class="col-md-4 mb-4">
                        <label for="" class="form-label">No. Kad Pengenalan Pemohon<span style="color:red">*</span></label>
                        <input
                          type="text"
                          :maxlength="12"
                          class="form-control"
                          placeholder="No. Kad Pengenalan"
                          v-model="nricno"
                          v-on:keypress="NumbersOnly"

                        />
                         <Error :message="nricerror" v-if="nricerror" />
                      </div>

                    </div>
                    <!-- close-row -->

                    <div class="row">

                          <div class="col-md-6 mb-4">
                            <label for="" class="form-label">Alamat Lengkap Pemohon</label>
                            <input type="text" class="form-control mb-3" placeholder="Alamat Lengkap" v-model="BranchAddress1" />

                            <input type="text" class="form-control mb-3" placeholder="Alamat Lengkap" v-model="BranchAddress2" />

                            <input type="text" class="form-control" placeholder="Alamat Lengkap" v-model="BranchAddress3" />
                          </div>

                          <div class="col-md-6">
          <label for="" class="form-label">Negeri</label>
          <select v-model="State" class="form-select" aria-label="Default select example" @change="onCitybind($event)">
            <option value="0">Sila Pilih</option>
            <option v-for="state in StateList" v-bind:key="state.id" v-bind:value="state.id">
              {{ state.state_name }}
            </option>
          </select>

          <div class="row mt-4">
            <div class="col-md-6">
              <label for="" class="form-label">Bandar</label>
              <select v-model="City" class="form-select" aria-label="Default select example"
                @change="onPostbind($event)">
                <option value="0">Sila Pilih</option>
                <option v-for="ctl in CityList" v-bind:key="ctl.city_name" v-bind:value="ctl.city_name">
                  {{ ctl.city_name }}
                </option>
              </select>
            </div>

            <div class="col-md-6">
              <label for="" class="form-label">Poskod</label>
              <select v-model="PostCode" class="form-select" aria-label="Default select example">
                <option value="0">Sila Pilih</option>
                <option v-for="pst in PostCodeList" v-bind:key="pst.id" v-bind:value="pst.id">
                  {{ pst.postcode }}
                </option>
              </select>
            </div>
          </div>
        </div>


                    </div>
                    <!-- close-row -->

                    <div class="row">
                      <div class="col-md-6 mb-4">
                        <label for="" class="form-label">Alamat Emel<span style="color:red">*</span></label>
                        <input
                          type="text"
                          class="form-control"
                          placeholder="Isi Alamat Emel"
                          v-model="email"
                          @blur="validateEmail"
                        />
                      </div>
                      <div class="col-md-3 mb-4">
                        <label for="" class="form-label">No. Telefon<span style="color:red">*</span></label>
                        <input
                          type="text"
                          :maxlength = "11"
                          oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);"
                          class="form-control"
                          placeholder="Isi No. Telefon."
                          v-model="contactno"
                          v-on:keypress="NumbersOnly"
                        />
                      </div>
                      <div class="col-md-3 mb-4">
                        <label for="" class="form-label">Peranan<span style="color:red">*</span></label>

                        <input
                          type="text"
                          :maxlength = "11"
                          oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);"
                          class="form-control"

                          v-model="rolelist2"
                          v-on:keypress="NumbersOnly"
                          disabled
                        />

                      </div>
                      </div>
                      <!-- close-row -->

                      <div class="row">
                      <!-- <div class="col-md-6 mb-4">
                        <label for="" class="form-label"
                          >ID Pengguna<span style="color:red">*</span></label
                        >
                        <input
                          type="text"
                          class="form-control"
                          placeholder="Isi ID Pengguna"
                          v-model="professionregno"
                        />
                      </div>
                      <div class="col-md-4 mb-4">
                        <label for="" class="form-label">Kata Laluan<span style="color:red">*</span></label>
                          <input
                            class="form-control"
                            id="inputPassword"
                            type="password"
                            placeholder="Isi Kata Laluan"
                            v-model="password"
                          />

                      </div> -->
                      <div class="row">
                      <div class="col-md-6 mb-4">
                        <label for="" class="form-label">Jenis Pemilik<span style="color:red">*</span></label>
                        <select
                          class="form-select"
                          aria-label="Default select example">
                          <option value="0">Sila Pilih</option>
                          <option></option>
                        </select>
                      </div>
                      </div>

                    </div>
                    <!-- close-row -->


                    <!-- close-row -->
 <p v-if="errors.length">
<ul>
        <li style="color:red"  v-for='err in errors'
    :key='err' >
          {{ err }}
        </li>
      </ul>
        </p>
        <br>
        <br>
                    <div class="form-foter mt-3">
                      <a
                        href="/app/modules/Admin/staff-management"
                        class="btn btn-primary btn-text"
                        ><i class="fa fa-arrow-alt-to-left"></i> Kembali</a
                      >
                      <div class="ml-auto" id="hidebutton" ref="hidebutton">
                        <button type="submit" class="btn btn-warning btn-text">
                          <i class="fa fa-save"></i> Simpan
                        </button>
                      </div>
                    </div>
                  </form>
                </div>
                   </div>

              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>
<script>
import CommonHeader from "../../../components/CommonHeader.vue";
import CommonSidebar from "../../../components/CommonSidebar.vue";
export default {
  components: { CommonSidebar, CommonHeader },
  name: "new-staff",
  data() {
    return {
      name: "",
      nricno: "",
      professionregno: "",
      roleId: 0,
      email: "",
      teamId: 0,
      contactno: "",
      designationId: 0,
      designationstartdate: "",
      designationenddate: "",
      branchId: 0,
      personincharge: 0,
      startdate: "",
      enddate: "",
      file: null,
      userdetails: null,
      rolelist: [],
      rolelist2: '',
      teamlist: [],
      designationlist: [],
      branchlist: [],
      errors: [],
      nricerror: null,
      SidebarAccess: null,
    };
  },
  beforeMount() {
    this.userdetails = JSON.parse(localStorage.getItem("userdetails"));
    this.SidebarAccess = JSON.parse(localStorage.getItem("SidebarAccess"));
    this.GetroleList();
    this.GetbranchList();
    this.GetdesignationList();
  },
  mounted() {
    if (this.SidebarAccess != 1) {
      this.$refs.hidebutton.classList.add("hide");
    }
  },
  methods: {
    async GetroleList() {
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get("roles/branch-viewlist", {
        headers,
      });
      this.rolelist = response.data.list;
      this.rolelist2 = response.data.list2[0].role_name;
      this.roleId = response.data.list2[0].id;
    },
    async GetteamList(branchId) {
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "service/getServiceListByBranch?branchId=" + branchId,
        {
          headers,
        }
      );
      if (response.data.code == 200 || response.data.code == "200") {
        this.teamlist = response.data.list;
      } else {
        this.teamlist = [];
      }
    },
    async GetbranchList() {
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get("hospital/branch-list", {
        headers,
      });
      if (response.data.code == 200 || response.data.code == "200") {
        this.branchlist = response.data.list;
      } else {
        this.branchlist = [];
      }
    },
    async GetdesignationList() {
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.get(
        "general-setting/list?section=" + "designation",
        {
          headers,
        }
      );
      if (response.data.code == 200 || response.data.code == "200") {
        this.designationlist = response.data.list;
      } else {
        this.designationlist = [];
      }
    },
    selectFile(event) {
      this.file = event.target.files[0];
    },
    onSelectBranch(event) {
      this.GetteamList(this.branchId);
    },
    async onCreateStaff() {

      this.$swal.fire({
        title: 'Are you sure to save this?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes, save it!',
        cancelButtonText: 'No, cancel!',
        reverseButtons: true
      }
      ).then(async (result) =>{
        if (result.isConfirmed){
        this.errors = [];
        if (!this.name) {
          this.errors.push("Name is required.");
        }
        if (!this.nricno) {
          this.errors.push("NRIC NO is required.");
        }

        if (this.roleId <= 0) {
          this.errors.push("Role  is required.");
        }
        if (!this.email) {
          this.errors.push("Email is required.");
        }
        if (this.teamId <= 0) {
          this.errors.push("Team  is required.");
        }
        if (!this.contactno) {
          this.errors.push("Contact NO is required.");
        }
        if (this.designationId <= 0) {
          this.errors.push("Designation  is required.");
        }
        if (!this.designationstartdate) {
          this.errors.push("Designation Period(Start Date) is required.");
        }
        if (this.branchId <= 0) {
          this.errors.push("Mentari Location is required.");
        }
        if (!this.startdate) {
          this.errors.push("Start Date is required.");
        }
        if (!this.file) {
          this.errors.push("Supported Document is required.");
        }

        console.log(
          "this.name && this.nricno &&  this.Role && this.email &&this.teamId && this.contactno && this.designationId && this.designationstartdate && this.designationenddate &&this.branchId && this.startdate && this.enddate && !this.nricerror",
          this.name,
          this.nricno,
          this.Role,
          this.email,
          this.teamId,
          this.contactno,
          this.designationId,
          this.designationstartdate,
          this.designationenddate,
          this.branchId,
          this.startdate,
          this.enddate,
          !this.nricerror
        );
        if (
          (this.name &&
          this.nricno &&
          this.roleId &&
          this.email &&
          this.teamId &&
          this.contactno &&
          this.designationId &&
          this.designationstartdate &&
          this.branchId &&
          this.startdate &&
          this.file &&
          !this.nricerror)
        ) {
          if (this.personincharge > 0) {
            this.personincharge = 1;
          }
          this.loader = true;
          const headers = {
            Authorization: "Bearer " + this.userdetails.access_token,
            Accept: "application/json",
            "Content-Type": "application/json",
          };
          let body = new FormData();
          body.append("added_by", this.userdetails.user.id);
          body.append("name", this.name);
          body.append("nric_no", this.nricno);
          body.append("registration_no", this.professionregno);
          body.append("role_id", this.roleId);
          body.append("email", this.email);
          body.append("team_id", this.teamId);
          body.append("branch_id", this.branchId);
          body.append("contact_no", this.contactno);
          body.append("designation_id", this.designationId);
          body.append("is_incharge", this.personincharge);
          body.append(
            "designation_period_start_date",
            this.designationstartdate
          );
          body.append("designation_period_end_date", this.designationenddate);
          body.append("start_date", this.startdate);
          body.append("end_date", this.enddate);
          body.append("document", this.file);
          const response = await this.$axios.post(
            "staff-management/addstaff",
            body,
            {
              headers,
            }
          );
          if (response.data.code == 200 || response.data.code == "200") {
            this.loader = false;
               await this.$swal.fire(
                                'Successfully Submitted.',
                                'Data is inserted.',
                                'success',
                              );
            this.$router.push("/Modules/Admin/staff-management");
          } else {
            this.loader = false;

            this.$swal.fire({
                            icon: 'error',
                            title: 'Oops... Something Went Wrong!',
                            text: 'the error is: ' + JSON.stringify(response.data.message),
                          });
          }
        }
      }
      })
    },

    async validateEmail() {
      if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(this.email)) {
        this.emailerror = null;
      } else {
        this.emailerror = "Please Enter Valid Email";
        this.email = "";
      }
    },
    async isLetter(e){
        let char = String.fromCharCode(e.keyCode);
        if(/^[A-Za-z\'@ ]+$/.test(char)) return true;
        else e.preventDefault();
    },

    NumbersOnly(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();;
      } else {
        return true;
      }
    },

    async CheckNric(event) {
      console.log("my body", event.target.value);
      const headers = {
        Authorization: "Bearer " + this.userdetails.access_token,
        Accept: "application/json",
        "Content-Type": "application/json",
      };
      const response = await this.$axios.post(
        "staff-management/checknricno",
        { nric_no: event.target.value },
        { headers }
      );
      console.log("response", response.data);
      if (response.data.code == 200) {
        this.nricerror = "NRIC No already exists";
      } else {
        this.nricerror = null;
      }
    },
  },
};
</script>
<style scoped>
.hide {
  display: none !important;
}
</style>
