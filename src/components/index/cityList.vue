<template>
  <div>
    <div>
      <mheader>选择城市</mheader>
    </div>
    <div class="wherePos">
      <span>定位您在</span>
      <span style="color: #ff510d; margin: 0 0.1rem">郑州市</span>
      <img style="width: 0.22rem; height: 0.26rem" src="../../assets/index/position.png"/>
    </div>
    <div class="main-body">
      <div class="main-go">
        <div>选择城市</div>
        <div style="width: 2px; height: 0.25rem; background-color: #ff682d; margin-left: 5px;"></div>
      </div>
      <div class="main-city" v-for="(item, index) in arrCity" :key="index">
        <div class="main-city-letter" v-if="index === 0" v-for="(item1, index) in item">
          <div>{{ item1.letter }}</div>
        </div>
        <div class="main-city-name">
          <div
            v-for="(item1, index1) in item"
            :key="index1"
            class="main-city-item"
            @click="cityItem(item1)"
            :style="{
            backgroundColor: tabIndex === item1 ? '#ff510d' : '#fff',
            borderColor: tabIndex === item1 ? '#ff510d' : '#ccc'
            }"
          >
            <div>{{ item1.city_name }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Mheader from '@/common/mheader'
  import { citys } from '@/api/allapi'
  import { sortList} from '../../../static/js/utils'

  export default {
    name: 'cityList',
    components: {
      Mheader,
      citys
    },
    data () {
      return {
        tabIndex: '',
        cityList: [
          {letter: 'A', cityArr: ['郑州1', '安阳2', '郑州3', '安阳4', '郑州5', '安阳6']},
          {letter: 'B', cityArr: ['郑州7', '安阳8']},
          {letter: 'C', cityArr: ['郑州9', '安阳10']},
          {letter: 'D', cityArr: ['郑州11', '安阳12']},
          {letter: 'E', cityArr: ['郑州13', '安阳14']},
          {letter: 'F', cityArr: ['郑州15', '安阳16']},
        ],
        arrCity: [],
        place: '', // 存放地址来源
        from: '', // 存放页面来源
      }
    },
    mounted () {
      this.place = this.$route.query.flag

      this.from = this.$route.query.from

      this._citys()
    },
    methods: {
      cityItem (val) {

        this.tabIndex = val

        // this.$router.push({path: '/', query: {value: val}})

        if (this.from === '乘客发布') {

          if (this.place === 'startPlace') {

            this.$router.push({path: '/passenger', query: {value1: val.city_name, startId: val.id}})

          } else if (this.place === 'endPlace') {

            this.$router.push({path: '/passenger', query: {value2: val.city_name, endId: val.id}})

          }

        } else if (this.from === '司机发布') {

          if (this.place === 'startPlace') {

            this.$router.push({path: '/driverPublish', query: {value1: val.city_name, startId: val.id}})

          } else if (this.place === 'endPlace') {

            this.$router.push({path: '/driverPublish', query: {value2: val.city_name, endId: val.id}})

          }

        } else {

          this.$router.go(-1)

          this.$toast.center('暂无来源')

        }

      // pinyin.getCamelChars("string") 此方法没什么用
      },
      _citys () {
        citys().then(res => {

          res.map(item => {

            let _letter = pinyin.getCamelChars(item.city_name).substring(0, 1)  // 取第一位字母

            item.letter = _letter

          })

          res.sort(
            function compareFunction(param1, param2) {
              return param1.letter.localeCompare(param2.letter,"en")
            }
          )

          this.arrCity = sortList(res)

          console.log('城市列表', res)

        })
      },
    }
  }
</script>

<style scoped>
  .wherePos {
    padding: 0.30rem;
    display: flex;
    justify-content: center;
    background-color: #ffffff;
    margin: 0.2rem 0;
  }
  .main-body {
    margin-top: 0.10rem;
    background-color: #ffffff;
  }
  .main-go {
    display: flex;
    font-size: 0.26rem;
    font-weight: bold;
    color: #333333;
    align-items: center;
    padding: 0.20rem 0.25rem;
    border-bottom: 1px solid #eaeaea;
  }
  .main-city {
    padding: 0 0.25rem;
  }
  .main-city-letter, .main-city-name {
    padding-top: 0.2rem;
  }
  .main-city-letter {
    font-size: 0.3rem;
    color: #ff510d;
    padding-left: 0.2rem;

  }
  .main-city-name {
    display: flex;
    flex-wrap: wrap;
    font-size: 0.26rem;
  }
  .main-city-item {
    width: 1.02rem;
    height: 0.38rem;
    border-radius: 0.1rem;
    border: 1px solid #ccc;
    text-align: center;
    line-height: 0.38rem;
    margin-right: 0.85rem;
    margin-top: 0.2rem;
  }
  .main-city-item:nth-child(4n+4) {
    margin-right: 0;
  }
</style>
