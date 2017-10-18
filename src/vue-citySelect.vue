
<template>

  <div>
    <select class="select-Type" :style="{height: height+'px'}" v-model="selectedCity.cityCode.provinceId" @change="getResult('province')">
      <option value="0">请选择省份</option>
      <option v-for="item in list" :value="item.id">{{item.name}}</option>
    </select>

    <select class="select-Type" :style="{height: height+'px'}" v-model="selectedCity.cityCode.cityId" @change="getResult('city')">
      <option value="0">请选择城市</option>
      <option v-show="city.length != 0" v-for="item in city" :value="item.id">{{item.name}}</option>
    </select>

    <select class="select-Type" :style="{height: height+'px'}" v-model="selectedCity.cityCode.districtId" @change="getResult('district')">
      <option value="0">请选择地区</option>
      <option v-show="district.length != 0" v-for="item in district"
              :value="item.id">{{item.name}}</option>
    </select>

  </div>

  <!-- 横向展示 纵向展示 -->
  <!-- 提示文字 在后面还是下面 -->
  <!-- {code:1-、1-2、1-2-3 ,codeText:河北-张家口市-尚义县} -->
  <!-- codeText:{县:商议,省:河北,市:张家口市} -->

</template>

<script>
import Configs from '../../lib/configs/ChinaCityList'
  export default {
    data () {
      return {
        // 市区数据列表
        city: [],
        // 县数据列表
        district: [],
        // 所有数据列表
        list: [],
        // 拼装数据格式
        selectedCity: {
          cityCode: {
            provinceId: 0,
            cityId: 0,
            districtId: 0,
          },
          cityText:{
            provinceText: '',
            cityText: '',
            districtText: '',
          },
          address: '',
        }
      }
    },
    mounted () {
       this.list = this.ConfigList;
    },
    methods: {
      getListItem (list, target, targetAttr) {
        for ( let i = 0; i < list.length; i++ ) {
          if ( list[i][targetAttr] == target ) {
            return list[i]
          }
        }
      },
      getResult (type) {
        var id = this.selectedCity.cityCode.provinceId,
       childId = this.selectedCity.cityCode.cityId;

        // 获取二级列表
        if ( type == 'province' ) {
          this.selectedCity.cityCode.districtId = 0
          this.selectedCity.cityCode.cityId = 0
          this.district = []
          var cityList = this.getListItem(this.list, id, 'id')
          typeof cityList != 'undefined' ? this.city = cityList.city : this.city = []
        }

        // 获取三级列表
        if ( type == 'city' ) {
          this.selectedCity.cityCode.districtId = 0
          var districtList = this.getListItem(this.city, childId , 'id')
          typeof districtList != 'undefined' ? this.district = districtList.district : this.district = []
        }
        // 组织emit数据
        var selectedCity = this.selectedCity
        // this.selectedCity.address =
      },
    },
    props: [ 'width', 'height', 'ConfigList' ]
  }
</script>

<style scoped>

</style>