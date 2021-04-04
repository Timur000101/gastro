<template>
  <div class="home">
    <div class="container">
      <div class="section">
        <div v-for="(item, index) in schedule" :key="index" class="section_item">
          <div class="section_item_switch">
            <label class="switch">
              <input type="checkbox" v-if="item.working === true" checked>
              <span class="slider round">Mon</span>
            </label>
          </div>
          <div class="section_item_body">
            <div class="time_input_label">Czas otwarcia</div>
            <div class="time_input">
              <input type="time" id="appt" name="appt"
                 :value="item.open" required>
            </div>
            <div class="line"></div>
            <div class="section_item_body_text">12 godz.</div>
            <div class="line"></div>
            <div class="time_input_label">Czas zamkicia</div>
            <div class="time_input">
              <input type="time" id="appt" name="appt"
               :value="item.close" required>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment';
moment.locale('ru')
export default {
  name: 'Home',
  data(){
    return {
      schedule: []
    }
  },
  mounted() {
    axios.post("http://3.121.84.2:1026/admin/testTask", {
      token: "wadgu29ufan2",
      password: "testTask"
    })
    .then(res => {
      for(let i = 0; i < res.data.schedule.length; i++) {
        let a = res.data.schedule[i].open;
        let b = res.data.schedule[i].close 
        if (a.toString().slice(1,2) == ':'){
          a = '0'+a
        }
        if (b.toString().slice(1,2) == ':'){
          b = '0'+b
        }
        this.schedule.push(
          { 
            "open": a,
            "working": res.data.schedule[i].working,
            "close": b 
          },
        )
      }
    })
    .catch(e => {
      console.log(e);
    })
  },
  methods:{

  }
}
</script>

<style scoped>
.home{
  background: #EDF8FC;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  /* width: 1200px; */
  margin: 0 auto;
  padding: 0 20px;
}
.section {
  display: flex;
  align-items: center;
  justify-content: space-around;
  flex-wrap: wrap;
  width: 100%;
}
.section_item{
  border: 2px solid #fff;
  border-radius: 40px;
  margin-right: 20px;
  margin-bottom: 20px;
}
.section_item:nth-of-type(7) {
  margin-right: 0;
}
.section_item_switch{
  margin: 20px;
}
.section_item_body{
  background: #D2E2F2;
  padding: 20px 40px;
  border-radius: 40px;
  width: calc(103% - 80px);
  margin-left: -1.5%;
}
.section_item_body_text{
  color: #667686;
  font-weight: 600;
}
.time_input_label{
  font-size: 16px;
  font-weight: bolder;
  padding-bottom: 10px;
}
.line{
  margin: 10px auto;
  height: 20px;
  width: 1px;
  border-left: 2px dotted #BDCDDD;
  text-align: center;
}
.time_input input{
  border-radius: 10px;
  border: none;
  outline: none;
  padding: 5px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 90px;
  height: 34px;
}

.switch input { 
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #4BDFAE;
}

input:focus + .slider {
  box-shadow: 0 0 1px #4BDFAE;
}

input:checked + .slider:before {
  -webkit-transform: translateX(56px);
  -ms-transform: translateX(56px);
  transform: translateX(56px);
}
.slider.round {
  border-radius: 34px;
  display: flex;
  align-items: center;
  padding: 0 10px;
  font-weight: bold;
}

.slider.round:before {
  border-radius: 50%;
}

@media (max-width: 483px) {
  .section {
    justify-content: center;
  }
}

</style>
