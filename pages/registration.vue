<template>
  <v-img
    :src="require('@/assets/images/3.jpg')"
    :lazy-src="require('@/assets/images/3.jpg')"
    class="fill-height"
    center
    width="100%"
    height="100%"
  >
    <v-container class="fill-height">
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="5">
          <!-- <v-card class="primary--border bg-card-color-opacity"> -->
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-card
              flat
              tile
              :style="styleObject"
              class="bg-card-color-opacity"
            >
              <v-card-text>
                <v-img
                  :src="require('@/assets/images/1.png')"
                  :lazy-src="require('@/assets/images/1.png')"
                  max-width="100%"
                  height="75"
                  contain
                ></v-img>
                <v-text-field
                  v-model="name"
                  prepend-icon="mdi-account"
                  :counter="100"
                  :rules="nameRules"
                  label="Name*"
                  required
                ></v-text-field>

                <v-text-field
                  v-model="email"
                  prepend-icon="mdi-email"
                  type="email"
                  label="Email*"
                  required
                  maxLength="100"
                  counter="100"
                  class="form-input"
                  :rules="emailRules"
                ></v-text-field>
                <div class="d-flex align-center">
                  <v-select
                    v-model="country"
                    outlined
                    label="Country Code*"
                    :rules="[(v) => !!v || 'Country Code is required']"
                    :items="countryCode"
                    style="width: 15px"
                  >
                  </v-select>
                  <v-text-field
                    v-model="mobileNo"
                    label="Mobile Number*"
                    type="number"
                    maxLength="10"
                    counter="10"
                    :rules="contactRules"
                    required
                    @focus="showText = true"
                    @blur="showText = false"
                  ></v-text-field>
                </div>
                <div v-if="showText" class="v-messages" role="alert">
                  <div class="text" style="padding-left: 23px">
                    <ul>
                      <li>
                        Please provide the correct mobile number as it will be
                        used for transaction verification, unlocking of account
                        etc.
                      </li>
                    </ul>
                  </div>
                </div>
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-menu
                      v-model="menu2"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="290px"
                    >
                      <template v-slot:activator="{ on }">
                        <v-text-field
                          v-model="dob"
                          label="Birth Date*"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-on="on"
                          :rules="dobRules"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="dob"
                        @input="menu2 = false"
                      ></v-date-picker>
                    </v-menu>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-select
                      v-model="gender"
                      label="Select Gender*"
                      prepend-icon="mdi-account"
                      :items="genderList"
                      :rules="[(v) => !!v || 'Gender is required']"
                      hide-details="auto"
                      required
                    ></v-select>
                  </v-col>
                </v-row>
                <v-text-field
                  v-model="password"
                  prepend-icon="mdi-lock"
                  :append-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
                  :type="showPassword ? 'text' : 'password'"
                  label="Password*"
                  required
                  maxLength="10"
                  :rules="passwordRules"
                ></v-text-field>
                <div
                  v-if="!isPasswordStrongFlag"
                  class="v-messages theme--dark error--text"
                  role="alert"
                >
                  <div class="v-messages__wrapper">
                    <div class="v-messages__message" style="padding-left: 23px">
                      Oops, this is one of the easy passwords to get hacked!
                      Please use a combination of numbers, alphabets and signs.
                    </div>
                  </div>
                </div>
                <div v-if="showPwdRules" class="v-messages" role="alert">
                  <div class="v-messages__wrapper">
                    <div class="v-messages__message" style="padding-left: 23px">
                      <ul>
                        <li>
                          Password must be min of 6 characters and max of 10.
                        </li>
                        <li>
                          Password must contain An upper case, a lower case, and
                          a number.
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
                <v-text-field
                  v-model="confirmPassword"
                  prepend-icon="mdi-lock"
                  :append-icon="showPassword1 ? 'mdi-eye-off' : 'mdi-eye'"
                  :type="showPassword1 ? 'text' : 'password'"
                  label="Confirm Password*"
                  required
                  maxLength="10"
                  :rules="confirmPasswordRules"
                ></v-text-field>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="referalCode"
                      :disabled="referalCodeInputDisable"
                      label="Referral Code"
                      prepend-icon="mdi-account-group"
                      required
                      @input="
                        referalCode != ''
                          ? (affiliateCodeInputDisable = true)
                          : (affiliateCodeInputDisable = false)
                      "
                      @click:append="showClose('referral')"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="affiliateCode"
                      :disabled="affiliateCodeInputDisable"
                      label="Affiliate Code"
                      prepend-icon="mdi-account-group"
                      required
                      @input="
                        affiliateCode != ''
                          ? (referalCodeInputDisable = true)
                          : (referalCodeInputDisable = false)
                      "
                      @click:append="showClose('affiliate')"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <div style="font-size: 16px">
                  <v-icon>mdi-bullhorn-outline</v-icon>
                  Where did you hear about FairPlay?*
                </div>
                <v-radio-group v-model="radioGroup">
                  <v-radio
                    v-for="value in obj"
                    :key="value"
                    :label="value"
                    :value="value"
                  ></v-radio>
                </v-radio-group>
                <v-textarea
                  v-model="box"
                  :disabled="
                    radioGroup == 'Any other source (Specify)' ? false : true
                  "
                  outlined
                  hide-details
                  class="rounded-lg"
                  rows="1"
                  maxLength="100"
                  counter="100"
                  onkeypress="return /^[a-zA-Z0-9\s]$/.test(event.key)"
                  placeholder="Any other source (Specify)"
                ></v-textarea>
                <div class="checkbox">
                  <input @click="EnableDisable" type="checkbox" />
                  <label
                    ><span class="check-box">
                      I'd like to receive promotional emails and
                      newsletter</span
                    ></label
                  >
                </div>
                <div class="checkbox">
                  <input @click="EnableDisable" type="checkbox" />
                  <label>
                    <span class="check-box"
                      >I am of legal age 18+ to gamble and I accept the
                      <v-dialog v-model="dialog" width="500">
                        <template v-slot:activator="{ on, attrs }">
                          <v-text
                            color="red lighten-2"
                            dark
                            v-bind="attrs"
                            v-on="on"
                          >
                            <a class="text-decoration-none"
                              >Terms and Conditions</a
                            >
                          </v-text>
                        </template>

                        <v-card>
                          <v-card-title class="text-h5 grey lighten-2">
                            
                          </v-card-title>

                          <v-card-text>
                            Lorem ipsum dolor sit amet, consectetur adipiscing
                            elit, sed do eiusmod tempor incididunt ut labore et
                            dolore magna aliqua. Ut enim ad minim veniam, quis
                            nostrud exercitation ullamco laboris nisi ut aliquip
                            ex ea commodo consequat. Duis aute irure dolor in
                            reprehenderit in voluptate velit esse cillum dolore
                            eu fugiat nulla pariatur. Excepteur sint occaecat
                            cupidatat non proident, sunt in culpa qui officia
                            deserunt mollit anim id est laborum.
                          </v-card-text>

                          <v-divider></v-divider>

                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" text @click="dialog = false">
                              Close
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                      and
                       <v-dialog v-model="dialog" width="500">
                        <template v-slot:activator="{ on, attrs }">
                          <v-text
                            color="red lighten-2"
                            dark
                            v-bind="attrs"
                            v-on="on"
                          >
                            <a class="text-decoration-none"
                              >Privacy Policy</a
                            >
                          </v-text>
                        </template>

                        <v-card>
                          <v-card-title class="text-h5 grey lighten-2">
                            Privacy Policy
                          </v-card-title>

                          <v-card-text>
                            Lorem ipsum dolor sit amet, consectetur adipiscing
                            elit, sed do eiusmod tempor incididunt ut labore et
                            dolore magna aliqua. Ut enim ad minim veniam, quis
                            nostrud exercitation ullamco laboris nisi ut aliquip
                            ex ea commodo consequat. Duis aute irure dolor in
                            reprehenderit in voluptate velit esse cillum dolore
                            eu fugiat nulla pariatur. Excepteur sint occaecat
                            cupidatat non proident, sunt in culpa qui officia
                            deserunt mollit anim id est laborum.
                          </v-card-text>

                          <v-divider></v-divider>

                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" text @click="dialog = false">
                              Close
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                      .</span
                    ></label
                  >
                </div>
                <v-btn
                  rounded
                  block
                  color="primary"
                  class="mt-4"
                  @click="saveFormData()"
                >
                  Register
                </v-btn>
                <v-card-text class="text-center">
                  Already a member?
                  <v-btn color="primary">Login</v-btn>
                </v-card-text>
              </v-card-text>
            </v-card>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </v-img>
</template>
<script>
export default {
  data: () => ({
    styleObject: { border: "3px solid orange" },

    valid: true,
    showText: false,
    genderList: ["Male", "Female", "Other"],
    countryCode: ["in", "ae", "us"],
    date: new Date().toISOString().substr(0, 10),
    menu2: false,
    showPassword: false,
    showPassword1: false,
    isPasswordStrongFlag: true,
    showPwdRules: false,
    referalCodeInputDisable: false,
    affiliateCodeInputDisable: false,
    saveClicked: false,
    dialog: false,
    obj: [
      "Ads",
      "OTT",
      "Instagram",
      "TV",
      "Friends Referral",
      "Banner",
      "Facebook",
      "Google Search",
      "Any other source (Specify)",
    ],

    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    country: "",
    mobileNo: "",
    contactRules: [
      (v) => !!v || "Mobile number is required",
      (v) => !(v && !/^[0-9]{8,10}$/.test(v)) || "Mobile Number is not valid",
    ],

    dob: "",
    dobRules: [(v) => !!v || "Birth Date is required"],
    gender: "",
    password: "",
    passwordRules: [
      (v) => !!v || "Password is required",
      (v) => !(v.length < 6) || "Password must be of 6 characters or more",
      (v) =>
        !(v.split(" ").join("").length < v.length) ||
        "Password cannot contain spaces!",
    ],
    confirmPassword: "",
    confirmPasswordRules: [
      (v) => !!v || "Confirm Password is required",
      (v) =>
        !(v.split(" ").join("").length < v.length) ||
        "Password cannot contain spaces!",
    ],
    referalCode: "",
    affiliateCode: "",
    radioGroup: "",
    box: "",
  }),

  methods: {
    validate() {
      this.$refs.form.validate();
    },
    saveFormData() {},
    EnableDisable() {
      if (this.form.checkedage) {
        this.valid = false;
      } else {
        this.valid = true;
      }
    },
  },
};
</script>
<style>
.bg-card-color-opacity {
  background-color: rgba(0, 0, 0, 0.75) !important;
}
</style>
