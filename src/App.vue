<template>
  <div id="container">
    <div id='front'>
      <div id="upper">
        <div id="goverment-vaccine" class="form-group">
          <label for="ic">選公費疫苗</label>
          <select class="form-control" id="ic" :size='goverment_vaccine.length' v-model="input.goverment_vaccine_selected">
            <option v-for='(vaccine,i) in goverment_vaccine' :key="i" :value="vaccine.key">{{vaccine.name}}</option>
          </select>
        </div>
        <div id="self-paid-vaccine" class="form-group" :size='selfpaid_vaccine.length'>
          <label for="self-paid-vaccine">選自費疫苗</label>
          <select multiple class="form-control" id="ic" size='5' v-model="input.selfpaid_vaccine_selected">
            <option v-for='(vaccine,i) in selfpaid_vaccine' :key="i" :value="vaccine.key">{{vaccine.name}}</option>
          </select>
        </div>
        <div id="identity" class="form-group" :size='identity.length'>
          <label for="identity">選看診身分</label>
          <select class="form-control" id="ic" size='5' v-model="input.identity_selected">
            <option v-for='(e,i) in identity' :key="i" :value="e.key">{{e.name}}</option>
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
              <input type="number" id="scanol-dose" class="form-control" v-model="input.selfpaid_scanol_dose">
              <label for="scanol-dose">ml q6h PRN</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="vit-d" v-model="input.selfpaid_vitd">
              <label class="form-check-label" for="vit-d">
                VitaD
              </label>
              <input type="number" id="vit-d-quantity" class="form-control" v-model="input.selfpaid_vitd_quantity">
              <label for="vit-d-quantity">瓶</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="multi-vita" v-model="input.selfpaid_multivita">
              <label class="form-check-label" for="multi-vita">
                VitaBaby
              </label>
              <input type="number" id="multi-vita-quantity" class="form-control" v-model="input.selfpaid_multivita_quantity">
              <label for="multi-vita-quantity">瓶</label>
            </div>
          </div>
        </div>
        <button id="submit" class="btn btn-secondary" @click="submit">submit</button>
      </div>
    </div>
    <div id="back">
      <div v-show="false">
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
        <div>selfpaid_vitd_quantity:{{input.selfpaid_vitd_quantity}}</div>
        <div>selfpaid_multivita:{{input.selfpaid_multivita}}</div>
        <div>selfpaid_multivita_quantity:{{input.selfpaid_multivita_quantity}}</div>
      </div>
      <h4>[output]</h4>
      <div>
        <h5>#IC01診(診間PA)</h5>
        <div>生效:{{opd.ic01_vaccine.show}}</div>
        <div>卡號:{{opd.ic01_vaccine.card_no}}</div>
        <div>身分:{{opd.ic01_vaccine.identity}}</div>
        <div>S:{{opd.ic01_vaccine.subjective}}</div>
        <div>O:{{opd.ic01_vaccine.objective}}</div>
        <div>P:{{opd.ic01_vaccine.plan}}</div>
        <div>診斷:{{opd.ic01_vaccine.diagnosis}}</div>
        <div>收費碼:{{opd.ic01_vaccine.order}}</div>
      </div>
      <div>
        <h5>#兒童健檢診(診間BB)</h5>
        <div>生效:{{opd.health_check.show}}</div>
        <div>卡號:{{opd.health_check.card_no}}</div>
        <div>身分:{{opd.health_check.identity}}</div>
        <div>S:{{opd.health_check.subjective}}</div>
        <div>O:{{opd.health_check.objective}}</div>
        <div>P:{{opd.health_check.plan}}</div>
        <div>診斷:{{opd.health_check.diagnosis}}</div>
        <div>收費碼:{{opd.health_check.order}}</div>
      </div>
      <div>
        <h5>#自費診(原診間)</h5>
        <div>生效:{{opd.self_paid_visit.show}}</div>
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
        { key: "GV00", isIC01: false, name: "無公費疫苗" },
        {
          key: "GV01",
          isIC01: true,
          name: "(IC01)HBV第二劑 1mo",
          order: ["96088S03", "960DRU39(HBV)(PART=II)"],
          injOrder: ["96188151(IM SelfPaid)", "M7001010(Syringe Needle)"]
        },
        {
          key: "GV02",
          isIC01: true,
          name: "(IC01)5in1 PCV-13(1) 2mo",
          order: [
            "96088S03",
            "960DRU59(Pediacel)(PART=I)",
            "960DRU40(PCV13)(PART=I@)"
          ],
          injOrder: [
            "96188151(IM SelfPaid)",
            "96188151(IM SelfPaid)",
            "M7001010(Syringe Needle)"
          ]
        },
        {
          key: "GV03",
          isIC01: true,
          name: "(IC01)5in1 PCV-13(2) 4mo",
          order: [
            "96088S03",
            "960DRU59(Pediacel)(PART=II)",
            "960DRU40(PCV13)(PART=II@)"
          ],
          injOrder: [
            "96188151(IM SelfPaid)",
            "96188151(IM SelfPaid)",
            "M7001010(Syringe Needle)"
          ]
        },
        {
          key: "GV04",
          isIC01: true,
          name: "(IC01)BCG 5mo",
          order: ["96088S03", "960DRU56(BCGJapan)"],
          injOrder: ["966DRUG0(BCG Injection)", "0007000T(Syringe,Insulin)"]
        },
        {
          key: "GV05",
          isIC01: true,
          name: "(IC01)6in1 6mo",
          order: ["96088S03", "960DRU43(Hexaxim)(PART=III)"],
          injOrder: ["96188151(IM SelfPaid)"]
        },
        {
          key: "GV06",
          isIC01: true,
          name: "(IC01)Varivax+MMR 1y",
          order: ["96088S03", "960DRU58(Varivax)", "960DRU45(PRIORIX)(PART=I)"],
          injOrder: [
            "96083665(SC INJECTION)",
            "M7001010(Syringe Needle)",
            "96083665(SC INJECTION)",
            "M7001010(Syringe Needle)"
          ]
        },
        { key: "GV07", isIC01: false, name: "PCV-13(4)+HAV 1y" }, //TODO
        {
          key: "GV08",
          isIC01: false,
          name: "5in1",
          order: ["960DRU59(Pediacel)(PART=IV)"],
          injOrder: ["96188151(IM SelfPaid)", "M7001010(Syringe Needle)"]
        },
        {
          key: "GV09",
          isIC01: false,
          name: "JE-IMO",
          order: ["960DRU50(JEV-IMO-JEV)"],
          injOrder: ["96083665(SC INJECTION)", "M7001010(Syringe Needle)"]
        },
        {
          key: "GV10",
          isIC01: false,
          name: "MMR(2) JE-IMO",
          order: [
            "960DRU50(JEV-IMO-JEV)(PART=@)",
            "960DRU45(PRIORIX)(PART=II)"
          ],
          injOrder: [
            "96083665(SC INJECTION)",
            "M7001010(Syringe Needle)",
            "96083665(SC INJECTION)",
            "M7001010(Syringe Needle)"
          ]
        },
        {
          key: "GV11",
          isIC01: false,
          name: "4in1 DTaP+IPV",
          order: ["960DRU42(Tetraxim)(PART=V)"],
          injOrder: ["96188151(IM SelfPaid)"]
        }
      ],
      selfpaid_vaccine: [
        { key: "SP00", name: "無自費疫苗" },
        { key: "SP01", name: "RotaTeq", order: "" },
        { key: "SP02", name: "RotaRix", order: "" },
        { key: "SP03", name: "RotaTeq 北市", order: "" },
        { key: "SP04", name: "RotaRix 北市", order: "" },
        { key: "SP05", name: "PCV-13", order: "" },
        { key: "SP06", name: "HAV", order: "" }
      ],
      identity: [
        { key: "ID00", name: "民眾" },
        {
          key: "ID01",
          name: "(71)兒童健檢",
          card_no: "健(71)",
          order: ["96000001", "961835AA"]
        },
        {
          key: "ID02",
          name: "(72)兒童健檢",
          card_no: "健(72)",
          order: ["96000002", "961835AB"]
        },
        {
          key: "ID03",
          name: "(73)兒童健檢",
          card_no: "健(73)",
          order: ["96000003", "961835AC"]
        },
        {
          key: "ID04",
          name: "(75)兒童健檢",
          card_no: "健(75)",
          order: ["96000004", "961835AE"]
        },
        {
          key: "ID05",
          name: "(76)兒童健檢",
          card_no: "健(76)",
          order: ["96000005", "961835AF"]
        },
        {
          key: "ID06",
          name: "(77)兒童健檢",
          card_no: "健(77)",
          order: ["96000006", "961835AH"]
        },
        {
          key: "ID07",
          name: "(79)兒童健檢",
          card_no: "健(79)",
          order: ["96000007", "961835AK"]
        }
      ],
      input: {
        goverment_vaccine_selected: "GV00",
        selfpaid_vaccine_selected: [],
        identity_selected: "ID00",
        height: "",
        weight: "",
        temperature: "",
        subjective: "",
        objective: "",
        plan: "",
        selfpaid_scanol: false,
        selfpaid_scanol_dose: "",
        selfpaid_vitd: false,
        selfpaid_vitd_quantity: "",
        selfpaid_multivita: false,
        selfpaid_multivita_quantity: ""
      },
      opd: {
        ic01_vaccine: {
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
    init: function() {
      let opd = this.opd;
      opd.ic01_vaccine.show = false;
      opd.ic01_vaccine.show = false;
      opd.ic01_vaccine.card_no = "";
      opd.ic01_vaccine.identity = "";
      opd.ic01_vaccine.subjective = "";
      opd.ic01_vaccine.objective = "";
      opd.ic01_vaccine.plan = "";
      opd.ic01_vaccine.diagnosis.splice(0, opd.ic01_vaccine.diagnosis.length);
      opd.ic01_vaccine.order.splice(0, opd.ic01_vaccine.order.length);

      opd.health_check.show = false;
      opd.health_check.card_no = "";
      opd.health_check.identity = "";
      opd.health_check.subjective = "";
      opd.health_check.objective = "";
      opd.health_check.plan = "";
      opd.health_check.diagnosis.splice(0, opd.health_check.diagnosis.length);
      opd.health_check.order.splice(0, opd.health_check.order.length);

      opd.self_paid_visit.show = false;
      opd.self_paid_visit.card_no = "";
      opd.self_paid_visit.identity = "";
      opd.self_paid_visit.subjective = "";
      opd.self_paid_visit.objective = "";
      opd.self_paid_visit.plan = "";
      opd.self_paid_visit.diagnosis.splice(
        0,
        opd.self_paid_visit.diagnosis.length
      );
      opd.self_paid_visit.order.splice(0, opd.self_paid_visit.order.length);
    },
    submit: function() {
      let vm = this;
      let input = this.input;
      let opd = this.opd;

      vm.init();

      let goverment_vaccine_selected = _.find(
        vm.goverment_vaccine,
        x => x.key == input.goverment_vaccine_selected
      ) || { isIC01: false };

      let identity_selected =
        _.find(vm.identity, x => x.key == input.identity_selected) || {};

      let selfpaid_vaccine_selected =
        _.map(input.selfpaid_vaccine_selected, x =>
          _.find(vm.selfpaid_vaccine, y => y.key == x)
        ) || {};

      //Block 0.計價自費疫苗及其他自費藥品
      //建立自費藥品收費清單
      let self_paid_drug = [];
      if (!_.find(selfpaid_vaccine_selected, x => x.key == "SP00")) {
        _.forEach(selfpaid_vaccine_selected, x => self_paid_drug.push(x.order));
      }

      //Block1. 根據看診身分(有無兒健檢)設定使用BB診或是民眾診
      if (identity_selected.key == "ID00") {
        //無兒健檢(民眾)
        opd.self_paid_visit.show = true;
        opd.self_paid_visit.diagnosis = ["V06.4_Z23"]; //自費診診斷為疫苗注射
        opd.self_paid_visit.identity = "民眾";
        opd.self_paid_visit.card_no = "無";
        //將公費疫苗相關注射處置掛在自費診下
        _.forEach(goverment_vaccine_selected.injOrder, x =>
          opd.self_paid_visit.order.push(x)
        );
      } else {
        //有兒健檢
        opd.health_check.show = true;
        opd.health_check.diagnosis = ["V20.2_Z00.129"];
        opd.health_check.identity = "健保";
        opd.health_check.card_no = identity_selected.card_no; //設定身分為對應的健7X卡號
        _.forEach(identity_selected.order, x => opd.health_check.order.push(x)); //將健檢處置碼加入BB
        //將公費疫苗注射治療處置掛在BB診下
        _.forEach(goverment_vaccine_selected.injOrder, x =>
          opd.health_check.order.push(x)
        );
        //如果有打疫苗(公費或自費)的話，BB診之診斷碼要加上疫苗注射
        if (
          goverment_vaccine_selected.key != "GV00" ||
          !_.find(selfpaid_vaccine_selected, x => x.key == "SP00")
        ) {
          opd.health_check.diagnosis.push("V06.4_Z23");
        }
      }

      //Block2. 公費疫苗選取狀態
      //-- 1. 無公費疫苗(GV00)
      //-- 2. IC01公費疫苗
      //-- 3. 非IC01公費疫苗

      if (goverment_vaccine_selected.key == "GV00") {
        //1. 無公費疫苗(GV00) do nothing
      } else if (goverment_vaccine_selected.isIC01) {
        //2. IC01公費疫苗
        opd.ic01_vaccine.show = true;
        opd.ic01_vaccine.card_no = "IC01";
        opd.ic01_vaccine.diagnosis = ["V06.4_Z23"];
        opd.ic01_vaccine.identity = "健保";
        //將疫苗藥物跟補助碼放入IC01
        _.forEach(goverment_vaccine_selected.order, x =>
          opd.ic01_vaccine.order.push(x)
        );
      } else if (!goverment_vaccine_selected.isIC01) {
        //3. 非IC01公費疫苗
        if (identity_selected.key == "ID00") {
          //無兒健檢-將疫苗藥物放入民眾
          _.forEach(goverment_vaccine_selected.order, x =>
            opd.self_paid_visit.order.push(x)
          );
        } else {
          //有兒健檢-將疫苗藥物放入BB
          _.forEach(goverment_vaccine_selected.order, x =>
            opd.health_check.order.push(x)
          );
        }
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
