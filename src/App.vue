<template>
  <div id="container">
    <div id='front'>
      <div id="upper">
        <div id="goverment-vaccine" class="form-group">
          <label for="ic">選公費疫苗</label>
          <select class="form-control" id="ic" :size='goverment_vaccine.length' v-model="input.goverment_vaccine_selected">
            <option value="">無</option>
            <option v-for='(vaccine,i) in goverment_vaccine' :key="i" :value="vaccine.key">{{vaccine.name}}</option>
          </select>
        </div>
        <div id="self-paid-vaccine" class="form-group" :size='selfpaid_vaccine.length'>
          <label for="self-paid-vaccine">選自費疫苗</label>
          <select multiple class="form-control" id="ic" size='5' v-model="input.selfpaid_vaccine_selected">
            <option value="">無</option>
            <option v-for='(vaccine,i) in selfpaid_vaccine' :key="i" :value="vaccine.key">{{vaccine.name}}</option>
          </select>
        </div>
        <div id="identity" class="form-group" :size='identity.length'>
          <label for="identity">選看診身分</label>
          <select class="form-control" id="ic" size='5' v-model="input.identity_selected">
            <option v-for='(e,i) in identity' :key="i" :value="e">{{e}}</option>
          </select>
        </div>
      </div>
      <div id="lower">
        <div id="measurements">
          <div class="form-group">
            <label for="bh">身高(cm)</label>
            <input type="number" min="0" class="form-control" id="bh" v-model="input.height">
          </div>
          <div class="form-group">
            <label for="bw">體重(kg)</label>
            <input type="number" min="0" class="form-control" id="bw" v-model="input.weight">
          </div>
          <div class="form-group">
            <label for="bt">體溫('C)</label>
            <input type="number" min="0" class="form-control" id="bt" v-model="input.temperature">
          </div>
        </div>
        <div id="left" class="col-container">
          <div class="form-group history">
            <label for="s">Subjective</label>
            <textarea type="number" class="form-control" id="s" rows="10" v-model="input.subjective"></textarea>
          </div>
          <div class="form-group history">
            <label for="o">Objective</label>
            <textarea type="number" class="form-control" id="o" rows="10" v-model="input.objective"></textarea>
          </div>
        </div>
        <div id="right" class="col-container">
          <div class="form-group history">
            <label for="p">Plan</label>
            <textarea type="number" class="form-control" id="p" rows="10" v-model="input.plan"></textarea>
          </div>
          <div id="additional-order">
            <label>自費醫囑</label>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="scanol" v-model="input.selfpaid_scanol">
              <label class="form-check-label" for="scanol">
                Scanol(self-paid)
              </label>
            </div>
            <div class="form-group" id="dose">
              <label for="scanol-dose">劑量</label>
              <input type="number" id="scanol-dose" class="form-control" v-model="input.selfpaid_scanol_dose">
              <label for="scanol-dose">ml q6h PRN</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="vit-d" v-model="input.selfpaid_vitd">
              <label class="form-check-label" for="vit-d">
                優寶D
              </label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="multi-vita" v-model="input.selfpaid_multivita">
              <label class="form-check-label" for="multi-vita">
                綜合維他命
              </label>
            </div>
          </div>
        </div>
        <button id="submit" class="btn btn-secondary" @click="submit">submit</button>
      </div>
    </div>
    <div id="back">
      <h4>[input]</h4>
      <div>goverment_vaccine:{{input.goverment_vaccine_selected}}</div>
      <div>selfpaid_vaccine:{{input.selfpaid_vaccine_selected}}</div>
      <div>identity:{{input.identity_selected}}</div>
      <div>height:{{input.height}}</div>
      <div>weight:{{input.weight}}</div>
      <div>temperature:{{input.temperature}}</div>
      <div>subjective:{{input.subjective}}</div>
      <div>objective:{{input.objective}}</div>
      <div>plan:{{input.plan}}</div>
      <div>selfpaid_scanol:{{input.selfpaid_scanol}}</div>
      <div>selfpaid_scanol_dose:{{input.selfpaid_scanol_dose}}</div>
      <div>selfpaid_vitd:{{input.selfpaid_vitd}}</div>
      <div>selfpaid_multivita:{{input.selfpaid_multivita}}</div>
      <h4>[output]</h4>
      <h5>#IC01診(診間PA)</h5>
      <div v-show="opd.goverment_vaccine.show">
        <div>卡號:{{opd.goverment_vaccine.card_no}}</div>
        <div>身分:{{opd.goverment_vaccine.identity}}</div>
        <div>S:{{opd.goverment_vaccine.subjective}}</div>
        <div>O:{{opd.goverment_vaccine.objective}}</div>
        <div>P:{{opd.goverment_vaccine.plan}}</div>
        <div>診斷:{{opd.goverment_vaccine.diagnosis}}</div>
        <div>收費碼:{{opd.goverment_vaccine.order}}</div>
      </div>
      <h5>#兒童健檢診(診間BB)</h5>
      <div v-show="opd.health_check.show">
        <div>卡號:{{opd.health_check.card_no}}</div>
        <div>身分:{{opd.health_check.identity}}</div>
        <div>S:{{opd.health_check.subjective}}</div>
        <div>O:{{opd.health_check.objective}}</div>
        <div>P:{{opd.health_check.plan}}</div>
        <div>診斷:{{opd.health_check.diagnosis}}</div>
        <div>收費碼:{{opd.health_check.order}}</div>
      </div>
      <h5>#自費診(原診間)</h5>
      <div v-show="opd.self_paid_visit.show">
        <div>卡號:{{opd.self_paid_visit.card_no}}</div>
        <div>身分:{{opd.self_paid_visit.identity}}</div>
        <div>S:{{opd.self_paid_visit.subjective}}</div>
        <div>O:{{opd.self_paid_visit.objective}}</div>
        <div>P:{{opd.self_paid_visit.plan}}</div>
        <div>診斷:{{opd.self_paid_visit.diagnosis}}</div>
        <div>收費碼:{{opd.self_paid_visit.order}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      goverment_vaccine: [
        { key: "GV01", isIC01: true, name: "(IC01)HBV第二劑 1mo" },
        { key: "GV02", isIC01: true, name: "(IC01)5in1 PCV-13(1) 2mo" },
        { key: "GV03", isIC01: true, name: "(IC01)5in1 PCV-13(2) 4mo" },
        { key: "GV04", isIC01: true, name: "(IC01)BCG 5mo" },
        { key: "GV05", isIC01: true, name: "(IC01)6in1 6mo" },
        { key: "GV06", isIC01: true, name: "(IC01)Varicella+MMR 1y" },
        { key: "GV07", isIC01: false, name: "PCV-13(4)+HAV 1y" },
        { key: "GV08", isIC01: false, name: "5in1" },
        { key: "GV09", isIC01: false, name: "MMR(2) JE-IMO" },
        { key: "GV10", isIC01: false, name: "4in1 DTaP+IPV" },
      ],
      selfpaid_vaccine: [
        {key:"SP01",name:"RotaTeq"},
        {key:"SP02",name:"RotaRix"},
        {key:"SP03",name:"RotaTeq 北市"},
        {key:"SP04",name:"RotaRix 北市"},
        {key:"SP05",name:"PCV-13"},
        {key:"SP06",name:"HAV"},
      ],
      identity: [
        "民眾",
        "(71)兒童健檢",
        "(72)兒童健檢",
        "(73)兒童健檢",
        "(75)兒童健檢",
        "(76)兒童健檢",
        "(77)兒童健檢",
        "(79)兒童健檢"
      ],
      input: {
        goverment_vaccine_selected: "",
        selfpaid_vaccine_selected: [],
        identity_selected: "",
        height: "",
        weight: "",
        temperature: "",
        subjective: "",
        objective: "",
        plan: "",
        selfpaid_scanol: false,
        selfpaid_scanol_dose: "",
        selfpaid_vitd: false,
        selfpaid_multivita: false
      },
      opd: {
        goverment_vaccine: {
          show: false,
          card_no: "",
          identity: "",
          subjective: "",
          objective: "",
          plan: "",
          diagnosis: [],
          order: []
        },
        health_check: {
          show: false,
          card_no: "",
          identity: "",
          subjective: "",
          objective: "",
          plan: "",
          diagnosis: [],
          order: []
        },
        self_paid_visit: {
          show: false,
          card_no: "",
          identity: "",
          subjective: "",
          objective: "",
          plan: "",
          diagnosis: [],
          order: []
        }
      }
    };
  },
  watch: {
    input: {
      handler: function(val, oldVal) {
        this.submit();
      },
      deep: true
    }
  },
  methods: {
    submit: function() {
      let input = this.input;
      let opd = this.opd;
      if (input.goverment_vaccine_selected.indexOf("IC01") !== -1) {
        opd.goverment_vaccine.show = true;
      } else {
        opd.goverment_vaccine.show = false;
      }
    }
  }
};
</script>

<style lang="scss">
@import "./assets/custom.scss";
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./style.scss";
</style>
