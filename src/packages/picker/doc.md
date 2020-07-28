# Picker 拾取器

提供多个选项集合供用户选择其中一项。

## 基本用法

```html
<nut-picker 
    :is-visible="isVisible1" 
    :default-value-data="defaultValueData1"
    :list-data="listData1"
    @close="switchPicker('isVisible1')"
    @confirm="setYearValue"
>
</nut-picker>
```
## 多列用法

```html
<nut-cell :showIcon="true" :isLink="true" @click.native="switchPicker('isVisible0')">
  <span slot="title">
    <label>年月选择</label>
  </span>
  <span slot="sub-title">不联动多列~~~</span>
  <div slot="desc" class="selected-option">{{ date ? date : '请选择' }}</div>
</nut-cell>
<nut-picker 
    :is-visible="isVisible0" 
    :list-data="listData0"
    title="请选择年月"
    :default-value-data="defaultValueData0"
    @close="switchPicker('isVisible0')"
    @confirm="setChooseValue0"
>
</nut-picker>
```

## 联动（省市区）

```html
<nut-picker 
    :is-visible="isVisible" 
    title="请选择城市"
    :list-data="listData"
    :default-value-data="defaultValueData"
    @close="switchPicker('isVisible')"
    @confirm="setChooseValue"
    @choose="updateChooseValue"
    @close-update="closeUpdateChooseValue"
>
</nut-picker>
```

## 联动（省市区）自定义数据

```html
<nut-picker
    :is-visible="isVisible2"
    title="请选择城市"
    :default-value-data="defaultValueData"
    :list-data="custmerCityData"
    @close="switchPicker('isVisible2')"
    @confirm="setChooseValueCustmer"
    @choose="updateChooseValueCustmer"
    @close-update="closeUpdateChooseValueCustmer"
></nut-picker>
```

```javascript
const APIData = [
  {
    label: 1,
    array: [
      {
        label: 3,
        value: "朝阳区"
      },
      {
        label: 4,
        value: "海淀区"
      }
    ]
  },
  {
    label: 2,
    array: [
      {
        label: 5,
        value: "测试1"
      },
      {
        label: 6,
        value: "测试2"
      }
    ]
  }
];
export default {
  data() {
    return {
      date: null,
      isVisible0: false,
      listData0: [
        [
          "2010",
          "2011",
          "2012",
          "2013",
          "2014",
          "2015",
          "2016",
          "2017",
          "2018",
          "2019",
          "2020",
          "2021",
          "2022",
          "2023",
          "2024",
          "2025",
          "2026",
          "2027",
          "2028",
          "2029",
          "2030",
          "2031",
          "2032",
          "2033",
          "2034",
          "2035",
          "2036",
          "2037",
          "2038",
          "2039"
        ],
        ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"]
      ],
      defaultValueData0: ["2012", "2"],
      city: null,
      isVisible: false,
      data: {
        北京: ["北京"],
        黑龙江: [
          "哈尔滨",
          "绥化",
          "漠河",
          "大兴安岭",
          "牡丹江",
          "佳木斯",
          "齐齐哈尔",
          "大庆",
          "五大连池"
        ],
        江西: ["九江", "南昌", "赣州"],
        上海: ["上海"],
        重庆: ["重庆"],
        内蒙古: [
          "呼和浩特",
          "呼和浩特1",
          "呼和浩特2",
          "呼和浩特3",
          "呼和浩特4",
          "呼和浩特5",
          "呼和浩特6",
          "呼和浩特7"
        ]
      },
      dataSub: {
        上海: ["测试1", "测试2"],
        北京: ["西城区", "东城区", "大兴区", "朝阳区", "海淀区"],
        南昌: ["青山湖区", "西湖区", "宏都中路", "梦时代", "八一广场"],
        绥化: ["明水", "拜泉"],
        哈尔滨: ["道里区", "道外区"]
      },
      listData: [["上海", "黑龙江", "北京", "重庆", "江西", "内蒙古"]],
      defaultValueData: null,
      year: null,
      isVisible1: false,
      listData1: [["2018", "2019","2010"]],
      custmerCityData: [
        [
          {
            label: 1,
            value: "北京"
          },
          {
            label: 2,
            value: "上海"
          }
        ]
      ],
      cityCustmer: null,
      isVisible2: false,
      defaultValueData1: null
    };
  },
  created() {
    this.listData = [
      ...[this.listData[0]],
      this.data[this.listData[0][0]],
      this.dataSub[this.data[this.listData[0][0]]]
    ];
  },
  methods: {
    switchPicker(param) {
      this[`${param}`] = !this[`${param}`];
    },

    setChooseValue0(chooseData) {
      this.date = `${chooseData[0]}年${chooseData[1]}月`;
    },

    setYearValue(chooseData) {
      this.year = `${chooseData[0]}年`;
    },

    modifyCity() {
      this.updateLinkage("", "重庆", 1, "重庆");
      this.defaultValueData = ["重庆", "重庆"];
    },

    modifyYear() {
      this.defaultValueData1 = ["2018"];
    },

    // demo 城市选择(联动) start
    setChooseValue(chooseData) {
      this.city = `${chooseData[0]}-${chooseData[1]}${
        chooseData[2] ? "-" + chooseData[2] : ""
      }`;
    },

    updateLinkage(self, value, index, chooseValue, cacheValueData) {
      if (!value) {
        return false;
      }
      switch (index) {
        case 1:
          let i = this.listData[0].indexOf(value);
          this.listData.splice(index, 1, [...this.data[this.listData[0][i]]]);
          chooseValue = chooseValue ? chooseValue : this.listData[index][0];
          self && self.updateChooseValue(self, index, chooseValue);
          this.updateLinkage(
            self,
            chooseValue,
            2,
            cacheValueData && cacheValueData[2] ? cacheValueData[2] : null
          );
          break;
        case 2:
          let areaData = this.dataSub[value] ? this.dataSub[value] : [];
          this.listData.splice(index, 1, [...areaData]);
          chooseValue = chooseValue ? chooseValue : this.listData[index][0];
          self && self.updateChooseValue(self, index, chooseValue);
          break;
      }
    },

    updateChooseValue(self, index, value, cacheValueData) {
      index < 2 && this.updateLinkage(self, value, index + 1, null);
    },

    closeUpdateChooseValue(self, chooseData) {
      this.updateLinkage(self, chooseData[0], 1, chooseData[1], chooseData);
    },
    // demo 城市选择(联动) end
    setChooseValueCustmer(chooseData) {
      //alert(JSON.stringify(chooseData));
      var str = chooseData.map(item => item.value).join("-");
      this.cityCustmer = str;
    },

    closeUpdateChooseValueCustmer(self, chooseData) {
        //此处模拟查询API，如果数据缓存了不需要再重新请求
        setTimeout(() => {
          let { label, value } = chooseData[0];
          var resItems = APIData.find(item => item.label == label);
          if (resItems && resItems.array.length) {
            this.$set(this.custmerCityData, 1, resItems.array);
            
            // 复原位置
            self.updateChooseValue(self, 0, chooseData[0]);
            self.updateChooseValue(self, 1, chooseData[1]);
          }
        }, 100);
    },

    updateChooseValueCustmer(self, index, resValue, cacheValueData) {
      // 本demo为二级联动，所以限制只有首列变动的时候触发事件
      if (index === 0) {
        //此处模拟查询API，如果数据缓存了不需要再重新请求
        let { label, value } = resValue;
        setTimeout(() => {
          var resItems = APIData.find(item => item.label == label);
          if (resItems && resItems.array.length) {
            this.$set(this.custmerCityData, 1, resItems.array);
            // 更新第二列位置
            self.updateChooseValue(self, index + 1, this.custmerCityData[1][0]);
          }
        }, 100);
      }
    }
  }
};
```

## Prop

| 字段 | 说明 | 类型 | 默认值
|----- | ----- | ----- | ----- 
| isVisible | 是否可见 | Boolean | false
| customClassName | 自定义class | String | null
| title | 设置标题 | String | null
| listData | 列表数据 | Array | []
| defaultValueData | 默认选中 | Array | []

## Event

| 字段 | 说明 | 回调参数 
|----- | ----- | ----- 
| confirm | 点击确认按钮时候回调 | 返回选中值
| choose | 每一列值变更时调用 | 依次返回this、改变的列数，改变值，当前选中值
| chclose-update | 联动时，关闭时回调 | 依次返回this、当前选中值
| close | 关闭时触发 | -