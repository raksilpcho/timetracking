<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">
        <div class="center">
          <div id="fontdev">Total Time</div>
          <!-- <div id="fontdev">hour</div> -->
          <el-table
            :data="rows"
            fit="true"
            size="small"
            border
            show-summary
            sum-text="TotalTime"
            style="width: 100%"
            table-layout="auto"
            max-height="100"
          >
            <el-table-column label="Total Time" />
            <el-table-column prop="time" label="Time" />
          </el-table>
        </div>
      </el-aside>
      <el-container>
        <!-- Header -->
        <el-header height="300px">
          <div class="hello">
            <h3>Log Time</h3>
            <el-form
              :model="ruleForm"
              :rules="rules"
              :label-position="labelPosition"
              label-width="100px"
              style="max-width: 460px"
            >
              <el-form-item label="Name" prop="name">
                <el-input v-model="ruleForm.name" />
                <!-- <div id="errorform" v-if="errorname">
                  {{ errorname }}
                </div> -->
              </el-form-item>
              <el-form-item label="" required>
                <el-col :span="11">
                  <el-form-item prop="date" label="Date">
                    <el-date-picker
                      v-model="ruleForm.date"
                      type="date"
                      label="Pick a date"
                      placeholder="Pick a date"
                      style="width: 100%"
                      value-format="YYYY-MM-DD"
                    />
                    <!-- <div id="errorform" v-if="errordate">
                  {{ errordate }}
                </div> -->
                  </el-form-item>
                </el-col>
                <el-col class="text-center" :span="2">
                  <span class="text-gray-500">-</span>
                </el-col>
                <el-col :span="11">
                  <el-form-item prop="time" label="Hours">
                    <el-input-number
                      v-model="ruleForm.time"
                      :precision="2"
                      :step="0.5"
                      :min="0"
                      :max="8"
                    />
                    <!-- <div id="errorform" v-if="errortime">
                      {{ errortime }}
                    </div> -->
                  </el-form-item>
                </el-col>
              </el-form-item>
              <el-form-item label="Comment" prop="comment">
                <el-input v-model="ruleForm.comment" />
                <!-- <div id="errorform" v-if="errorcomment">
                  {{ errorcomment }}
                </div> -->
              </el-form-item>

              <!-- Button -->
              <el-form-item>
                <!-- <el-button type="primary" @click="addRow(ruleForm)" -->
                <el-button type="primary" @click="submitForm(ruleForm, rules)"
                  >Save</el-button
                >
                <el-button type="danger" @click="resetForm('ruleForm')"
                  >Cancel</el-button
                >
              </el-form-item>
            </el-form>
          </div>
        </el-header>
        <!-- main  <h2 v-for="result in dbs" :key="result.name">{{result.name}}</h2> -->
        <el-main>
          <el-table :data="rows" height="250" border style="width: 100%">
            <el-table-column prop="name" label="Name" width="180">
              <template #slot-scope="scope">
                <img :src="`/uploads/${scope.row.image}`" width="50px" />
              </template>
              <!-- <div id="app">
                <img src="../assets/profile.png" /> 
              </div> -->
            </el-table-column>
            <el-table-column prop="date" label="Date" width="180" />
            <el-table-column prop="time" label="Time" />
            <el-table-column prop="comment" label="Comment" />
          </el-table>
        </el-main>

        <!-- Footer -->
        <el-footer> </el-footer>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import { reactive, ref, computed } from "vue";
import db from "../../data/db.json";

export default {
  setup() {
    // vailidation
    let input = ref("");
    const errorname = computed(() => {
      return ruleForm.name === "" ? "Please input Activity name" : "";
    });
    const errordate = computed(() => {
      return ruleForm.date === "" || ruleForm.date === null
        ? "The Input field is required"
        : "";
    });
    const errortime = computed(() => {
      return ruleForm.time === 0 || ruleForm.date === ""
        ? "The Input field is required"
        : "";
    });
    const errorcomment = computed(() => {
      return ruleForm.comment === "" ? "The Input field is required" : "";
    });

    const rules = {
      name: [
        {
          required: true,
          message: "Please input Activity name",
          trigger: "blur",
        },
        { min: 2, message: "Length should be minimun 2", trigger: "blur" },
        {
          required: true,
          message: "Please input Activity name",
          trigger: "change",
        },
      ],
      date: [
        {
          type: "date",
          required: true,
          message: "Please pick a date",
          trigger: "change",
        },
      ],
      time: [
        {
          type: "number",
          required: true,
          message: "Please pick a time",
          trigger: "change",
        },
        { min: 1, message: "Length should not negative", trigger: "blur" },
      ],
      comment: [
        {
          required: true,
          message: "Please input activity form",
          trigger: "blur",
        },
        {
          required: true,
          message: "Please input activity form",
          trigger: "change",
        },
      ],
    };

    const submitForm = (ruleForm, rules) => {
      // ruleForm.validate((valid) => {
      if (ruleForm.name === "") {
        rules.name;
        alert("Please input activity form name field");
      } else if (ruleForm.date === "" || ruleForm.date === null) {
        alert("Please pick a date");
      } else if (
        ruleForm.time === 0 ||
        ruleForm.date === "" ||
        ruleForm.date < 1
      ) {
        alert("Please pick a time");
      } else if (ruleForm.comment === "") {
        alert("Please input activity form comment field");
      }
      // else if (valid) {
      else {
        rows.value.push({
          name: ruleForm.name,
          date: ruleForm.date,
          time: ruleForm.time,
          comment: ruleForm.comment,
        });

        return false;
      }
      //  });
    };

    const labelPosition = ref("top");
    const formLabelAlign = reactive({
      name: "",
      date: "",
      time: "",
      comment: "",
    });

    const initialState = {
      name: "",
      date: "",
      time: "",
      comment: "",
    };
    const ruleForm = reactive({ ...initialState });

    // reset
    function resetForm() {
      Object.assign(ruleForm, initialState);
    }
    // addrow
    const rows = ref([]);

    return {
      labelPosition,
      formLabelAlign,
      ruleForm,
      resetForm,
      initialState,
      rows,
      rules,
      dbs: db.ruleForm,
      // vaild
      input,
      errorname,
      errordate,
      errortime,
      errorcomment,
      submitForm,
    };
  },
  created() {
    window.addEventListener("beforeunload", alert("Refresh"));
  },
  mounted() {
    //  const data = JSON.stringify(this.rows)
    //       window.localStorage.setItem('rows', data);
    //  if (localStorage.getItem("rows") === null) {
    //   //  alert("create")
    //     this.rows = JSON.parse(localStorage.getItem("rows"));
    //     localStorage.setItem("rows", JSON.stringify(this.rows));
    //   }
  },
};
</script>

<style scoped>
h3 {
  margin: 10px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
img {
  height: 120px;
  width: 150px;
}
.center {
  line-height: 50px;
  height: 100%;
  background-color: #42b983;
}
#errorform {
  color: red;
}
#fontdev {
  color: black;

  font-size: 20px;
  font-weight: bold;
  border: 3px solid black;
  padding: 10px;
  text-align: center;
}
</style>
