<template>
  <v-container id="machine-learning" fluid tag="section">
    <v-row justify="center">
      <v-col cols="12" md="8">
        <base-material-card
          icon="mdi-robot"
          title="Machine Learning Prediction for Kickstarter Projects"
          class="px-5 py-3"
        >
          <v-form>
            <v-container class="py-0">
              <v-row>
                <v-col cols="12">
                  <v-text-field v-model="projectName" label="Your Project's Name"></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                  <v-select
                    v-model="mainCategory"
                    :items="mainCategoryList"
                    label="Main Category"
                    prepend-icon="mdi-shape"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" md="6">
                  <v-select
                    v-model="country"
                    :items="countryList"
                    label="Country"
                    prepend-icon="mdi-earth"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="4">
                  <v-subheader>Funding goal in USD</v-subheader>
                </v-col>
                <v-col cols="8">
                  <v-text-field
                    v-model="usdGoal"
                    label="Amount"
                    prefix="$"
                    :rules="isNumberRules"
                    prepend-icon="mdi-flag-checkered"
                  ></v-text-field>
                </v-col>
                <v-col cols="4">
                  <v-subheader>Launched Date</v-subheader>
                </v-col>
                <v-col cols="8">
                  <v-dialog
                    ref="launchedDialog"
                    v-model="launchedDateModal"
                    :return-value.sync="launchedDate"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="launchedDate"
                        label="Launched"
                        prepend-icon="mdi-rocket"
                        readonly
                        v-on="on"
                        required
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="launchedDate" scrollable>
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="launchedDateModal = false">Cancel</v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.launchedDialog.save(launchedDate)"
                      >OK</v-btn>
                    </v-date-picker>
                  </v-dialog>
                </v-col>

                <v-col cols="4">
                  <v-subheader>Deadline Date</v-subheader>
                </v-col>
                <v-col cols="8">
                  <v-dialog
                    ref="deadlineDialog"
                    v-model="deadlineDateModal"
                    :return-value.sync="deadlineDate"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        v-model="deadlineDate"
                        label="Deadline"
                        prepend-icon="mdi-alert-circle"
                        readonly
                        v-on="on"
                        required
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="deadlineDate" scrollable>
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="deadlineDateModal = false">Cancel</v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.deadlineDialog.save(deadlineDate)"
                      >OK</v-btn>
                    </v-date-picker>
                  </v-dialog>
                </v-col>

                <v-col cols="12" class="text-right">
                  <v-btn color="primary" class="mr-0" @click="onPredict()">Start Prediction!</v-btn>
                  <!-- Dialog for Prediction Result -->
                  <v-dialog v-model="predictionDialog" max-width="290">
                    <v-card>
                      <v-card-title class="headline">{{ predictionTitle }}</v-card-title>

                      <v-card-text class="text-center">
                        Your Project
                        <strong>{{ projectName }}</strong> has
                      </v-card-text>
                      <v-card-text class="text-center">
                        <h1>{{ prediction }} %</h1>
                      </v-card-text>
                      <v-card-text class="text-center">probability of beeing Successful</v-card-text>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="primary" class="mr-0" @click="resetPredictionDialog()">Got It</v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-dialog>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
        </base-material-card>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
import { mapGetters, mapActions, mapMutations } from "vuex";
export default {
  name: "MachineLearning",
  data: () => ({
    projectName: "",
    mainCategory: "",
    country: "",
    usdGoal: 0,
    launchedDateModal: false,
    launchedDate: new Date().toISOString().substr(0, 10),
    deadlineDateModal: false,
    deadlineDate: new Date().toISOString().substr(0, 10),
    mainCategoryList: [
      "Art",
      "Comics",
      "Crafts",
      "Dance",
      "Design",
      "Fashion",
      "Film & Video",
      "Food",
      "Games",
      "Journalism",
      "Music",
      "Photography",
      "Publishing",
      "Technology",
      "Theater",
    ],
    countryList: ["US", "GB", "Other"],
    isNumberRules: [
      (v) => !!v || "Number is required",
      (v) => /^\d+$/.test(v) || "Number of backers must be a number",
      (v) => v >= 0 || "Number of backers must be greater than cero",
    ],
  }),

  computed: {
    ...mapGetters("machineLearning", {
      prediction: "getPrediction",
      predictionDialog: "getPredictionDialog",
    }),
    fundedRatio() {
      return this.usdPledged / this.usdGoal;
    },
    period() {
      const deadlineDate = new Date(this.deadlineDate);
      const launchedDate = new Date(this.launchedDate);
      const differenceInTime = deadlineDate.getTime() - launchedDate.getTime();
      const period = differenceInTime / (1000 * 3600 * 24);
      return period;
    },
    predictionTitle() {
      return this.prediction > 50 ? "Congratulations!" : "Sorry";
    },
  },

  watch: {},

  created() {},

  methods: {
    ...mapActions("machineLearning", ["predict"]),
    ...mapMutations("machineLearning", ["resetPredictionDialog"]),
    onPredict() {
      const projectFormData = {
        main_category: this.mainCategory,
        country: this.country,
        usd_goal_real: parseFloat(this.usdGoal),
        period: this.period,
      };
      this.predict(projectFormData);
    },
  },
};
</script>
