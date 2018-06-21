<template>
  <div id="container">
    <div id='front'>
      <div id="upper">
        <div id="goverment-vaccine" class="form-group">
          <label for="ic">選公費疫苗</label>
          <select class="form-control" id="ic" :size='goverment_vaccine.length' v-model="goverment_vaccine_selected">
            <option value="">無</option>
            <option v-for='(vaccine,i) in goverment_vaccine' :key="i" :value="vaccine">{{vaccine}}</option>
          </select>
        </div>
        <div id="self-paid-vaccine" class="form-group" :size='selfpaid_vaccine.length'>
          <label for="self-paid-vaccine">選自費疫苗(optional)</label>
          <select multiple class="form-control" id="ic" size='5' v-model="selfpaid_vaccine_selected">
            <option value="">無</option>
            <option v-for='(vaccine,i) in selfpaid_vaccine' :key="i" :value="vaccine">{{vaccine}}</option>
          </select>
        </div>
        <div id="identity" class="form-group" :size='identity.length'>
          <label for="identity">選看診身分</label>
          <select class="form-control" id="ic" size='5' v-model="identity_selected">
            <option v-for='(e,i) in identity' :key="i" :value="e">{{e}}</option>
          </select>
        </div>
      </div>
      <div id="lower">
        <div id="measurements">
          <div class="form-group">
            <label for="bh">身高(cm)</label>
            <input type="number" min="0" class="form-control" id="bh" v-model="height">
          </div>
          <div class="form-group">
            <label for="bw">體重(kg)</label>
            <input type="number" min="0" class="form-control" id="bw" v-model="weight">
          </div>
          <div class="form-group">
            <label for="bt">體溫('C)</label>
            <input type="number" min="0" class="form-control" id="bt" v-model="temperature">
          </div>
        </div>
        <div id="left" class="col-container">
          <div class="form-group history">
            <label for="s">Subjective</label>
            <textarea type="number" class="form-control" id="s" rows="10" v-model="subjective"></textarea>
          </div>
          <div class="form-group history">
            <label for="o">Objective</label>
            <textarea type="number" class="form-control" id="o" rows="10" v-model="objective"></textarea>
          </div>
        </div>
        <div id="right" class="col-container">
          <div class="form-group history">
            <label for="p">Plan</label>
            <textarea type="number" class="form-control" id="p" rows="10" v-model="plan"></textarea>
          </div>
          <div id="additional-order">
            <label>自費醫囑</label>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="scanol" v-model="selfpaid_scanol">
              <label class="form-check-label" for="scanol">
                Scanol(self-paid)
              </label>
            </div>
            <div class="form-group" id="dose">
              <label for="scanol-dose">劑量</label>
              <input type="number" id="scanol-dose" class="form-control" v-model="selfpaid_scanol_dose">
              <label for="scanol-dose">ml q6h PRN</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="vit-d" v-model="selfpaid_vitd">
              <label class="form-check-label" for="vit-d">
                優寶D
              </label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" value="" id="multi-vita" v-model="selfpaid_multivita">
              <label class="form-check-label" for="multi-vita">
                綜合維他命
              </label>
            </div>
          </div>
        </div>
        <button id="submit" class="btn btn-secondary">submit</button>
      </div>
    </div>
    <div id="back">
      <h4>[input]</h4>
      <div>goverment_vaccine:{{goverment_vaccine_selected}}</div>
      <div>selfpaid_vaccine:{{selfpaid_vaccine_selected}}</div>
      <div>identity:{{identity_selected}}</div>
      <div>height:{{height}}</div>
      <div>weight:{{weight}}</div>
      <div>temperature:{{temperature}}</div>
      <div>subjective:{{subjective}}</div>
      <div>objective:{{objective}}</div>
      <div>plan:{{plan}}</div>
      <div>selfpaid_scanol:{{selfpaid_scanol}}</div>
      <div>selfpaid_scanol_dose:{{selfpaid_scanol_dose}}</div>
      <div>selfpaid_vitd:{{selfpaid_vitd}}</div>
      <div>selfpaid_multivita:{{selfpaid_multivita}}</div>
      <h4>[output]</h4>
      <h5>#IC01診(診間PA)</h5>
      <div>卡號:</div>
      <div>身分:</div>
      <div>S:</div>
      <div>O:</div>
      <div>P:</div>
      <div>診斷:</div>
      <div>收費碼:</div>
      <h5>#兒童健檢診(診間BB)</h5>
      <div>卡號:</div>
      <div>身分:</div>
      <div>S:</div>
      <div>O:</div>
      <div>P:</div>
      <div>診斷:</div>
      <div>收費碼:</div>
      <h5>#自費診(原診間)</h5>
      <div>卡號:</div>
      <div>身分:</div>
      <div>S:</div>
      <div>O:</div>
      <div>P:</div>
      <div>診斷:</div>
      <div>收費碼:</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      goverment_vaccine: ["IC01-1", "IC01-2", "IC01-3"],
      selfpaid_vaccine: ["RotaTeq", "RotaRix", "PCV-13"],
      identity: ["民眾", "(71)兒童健檢", "(72)兒童健檢", "(73)兒童健檢"],
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
    };
  }
};
</script>

<style lang="scss">
@import "./assets/custom.scss";
@import "../node_modules/bootstrap/scss/bootstrap.scss";
@import "./style.scss";
</style>
