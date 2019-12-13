<style scoped>
/*@media screen and (min-width: 460px) {
  .calendar_item_date:hover {
    background: #71c7a5;
    cursor: pointer;
  }
}*/
* {
  margin: 0;
  padding: 0;
}
.calendar_container {
  /*max-width: 410px;*/
  margin: auto;
}
li {
  list-style-type: none;
}
.calendar_top_changge {
  display: flex;
}
.calendar_top_changge .li {
  /*cursor: pointer;*/
  display: flex;
  color: #333333;
  font-size: 32px;
  flex: 1;
  justify-content: center;
  align-items: center;
  height: 70px;
}
.calendar_top_changge .li1 {
  justify-content: flex-end;
}
.calendar_top_changge .li2 {
  justify-content: flex-start;
}
.calendar_top_changge .calendar_content_li {
  cursor: auto;
  flex: 1;
}
.calendar_content_all {
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
    "Helvetica Neue", STHeiti, "Microsoft Yahei", Tahoma, Simsun, sans-serif;
  /*background-color: #0fc37c;*/
  width: 100%;
  overflow: hidden;
  padding-bottom: 8px;
}
.calendar_content {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}
.calendar_content:first-child .calendar_content_item_tag,
.calendar_content:first-child .calendar_content_item {
  color: #ddd;
  font-size: 32px;
}
.calendar_content_item, .calendar_content_item_tag {
  font-size: 32px;
  width: 14.28%;
  text-align: center;
  color: #fff;
  position: relative;
}
.calendar_content_item {
  height: 60px;
  margin: 10px 0 20px;
}
.calendar_content_item .calendar_today_status {
  display: block;
  position: absolute;
  left: 44%;
  bottom: -30%;
  width: 12px;
  height: 12px;
  border-radius: 6px;
  background: #EF7625;
  /*background: red;*/
}
.calendar_top_tag {
  width: 60px;
  height: 60px;
  line-height: 60px;
  color: #3E3C3D;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calendar_item_date {
  width: 60px;
  height: 60px;
  line-height: 60px;
  border-radius: 50%;
  background: #DEDFE2;
  color: #fff;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calendar_item_date_num {
  position: absolute;
  z-index: 5;
  left: 22%;
  top: 1.8%;
  width: 60px;
  height: 60px;
  line-height: 60px;
  border-radius: 50%;
  font-size: 32px;
  color: #fff;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calendar_jiantou1 {
  width: 12px;
  height: 12px;
  border-top: 2px solid #333;
  border-left: 2px solid #333;
  transform: rotate(-45deg);
}
.triangle_border_left{
  width:0;
  height:0;
  border-width:10px 10px 10px 0;
  border-style:solid;
  border-color:transparent #333 transparent transparent;
  position:relative;
}
.triangle_border_right{
  width:0;
  height:0;
  border-width:10px 0 10px 10px;
  border-style:solid;
  border-color:transparent transparent transparent #333;
  position:relative;
}
/*.calendar_jiantou1:active,
.calendar_jiantou2:active {
  border-color: #ddd;
}*/
.calendar_jiantou2 {
  width: 12px;
  height: 12px;
  border-top: 2px solid #333;
  border-right: 2px solid #333;
  transform: rotate(45deg);
}
.calendar_content_item > .calendar_isMark {
  margin: auto;
  border-radius: 100px;
  background: blue;
  z-index: 2;
}
.calendar_content_item .calendar_other_dayhide {
  color: #bfbfbf;
}
.calendar_content_item .calendar_want_dayhide {
  color: #bfbfbf;
}
/*.calendar_content_item .calendar_isToday {
  background: yellow;
  color: yellow;
  border-radius: 100px;
}*/
.calendar_content_item .calendar_chose_day {
  /*background: green;*/
  position: absolute;
  left: 22%;
  top: 0%;
  z-index: 2;
  background: #000;
  opacity: 0.2;
  width: 60px;
  height: 60px;
  line-height: 60px;
  border-radius: 100px;
}
</style>
<template>
  <section class="calendar_container">
    <div class="calendar_content_all">
      <div class="calendar_top_changge">
        <div class="li li1" @click="PreMonth(myDate,false)">
          <!-- <div class="calendar_jiantou1"></div> -->
            <!-- 向左的三角形 -->
            <div class="triangle_border_left">
            </div>
        </div>
        <div class="calendar_content_li li">{{dateTop}}</div>
        <div class="li li2" @click="NextMonth(myDate,false)">
          <!-- <div class="calendar_jiantou2"></div> -->
            <div class="triangle_border_right">
            </div>
        </div>
      </div>
      <div class="calendar_content">
        <div class="calendar_content_item" v-for="tag in textTop" :key="tag">
          <div class="calendar_top_tag">{{tag}}</div>
        </div>
      </div>
      <div class="calendar_content">
        <div class="calendar_content_item" v-for="(item, index) in list" @click="clickDay(item, index)" :key="index">
          <div
            class="calendar_item_date"
            v-bind:class="[{ calendar_isMark: item.isMark},{calendar_other_dayhide:item.otherMonth!=='nowMonth'},{calendar_want_dayhide:item.dayHide},setClass(item)]"
          ></div>
          <div class="calendar_item_date_num">{{item.id}}</div>
          <div v-bind:class="{calendar_chose_day:item.chooseDay}"></div>
          <span v-bind:class="{calendar_today_status:item.isToday}"></span>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import timeUtil from './calendar'
export default {
    data () {
        return {
            myDate: [],
            list: [],
            historyChose: [],
            dateTop: ''
        }
    },
    props: {
        markDate: {
            type: Array,
            default: () => []
        },
        markDateMore: {
            type: Array,
            default: () => []
        },
        textTop: {
            type: Array,
            default: () => ['日', '一', '二', '三', '四', '五', '六']
        },
        sundayStart: {
            type: Boolean,
            default: () => false
        },
        agoDayHide: {
            type: String,
            default: `0`
        },
        futureDayHide: {
            type: String,
            default: `2554387200`
        },
        selectDate: {
            type: String,
            default: `2554387200`
        }
    },
    created () {
        this.intStart()
        this.myDate = new Date(this.selectDate)
    },
    methods: {
        intStart () {
            timeUtil.sundayStart = this.sundayStart
        },
        setClass (data) {
            let obj = {}
            obj[data.markClassName] = data.markClassName
            return obj
        },
        clickDay: function (item, index) {
            if (item.otherMonth === 'nowMonth' && !item.dayHide) {
                this.getList(this.myDate, item.date)
            }
            if (item.otherMonth !== 'nowMonth') {
                item.otherMonth === 'preMonth'
                    ? this.PreMonth(item.date)
                    : this.NextMonth(item.date)
            }
        },
        ChoseMonth: function (date, isChosedDay = true) {
            date = timeUtil.dateFormat(date)
            this.myDate = new Date(date)
            this.$emit('changeMonth', timeUtil.dateFormat(this.myDate))
            if (isChosedDay) {
                this.getList(this.myDate, date, isChosedDay)
            } else {
                this.getList(this.myDate)
            }
        },
        PreMonth: function (date, isChosedDay = true) {
            date = timeUtil.dateFormat(date)
            this.myDate = timeUtil.getOtherMonth(this.myDate, 'preMonth')
            this.$emit('changeMonth', timeUtil.dateFormat(this.myDate))
            if (isChosedDay) {
                this.getList(this.myDate, date, isChosedDay)
            } else {
                this.getList(this.myDate)
            }
        },
        NextMonth: function (date, isChosedDay = true) {
            date = timeUtil.dateFormat(date)
            this.myDate = timeUtil.getOtherMonth(this.myDate, 'nextMonth')
            this.$emit('changeMonth', timeUtil.dateFormat(this.myDate))
            if (isChosedDay) {
                this.getList(this.myDate, date, isChosedDay)
            } else {
                this.getList(this.myDate)
            }
        },
        forMatArgs: function () {
            let markDate = this.markDate
            let markDateMore = this.markDateMore
            markDate = markDate.map(k => {
                return timeUtil.dateFormat(k)
            })
            markDateMore = markDateMore.map(k => {
                k.date = timeUtil.dateFormat(k.date)
                return k
            })
            return [markDate, markDateMore]
        },
        getList: function (date, chooseDay, isChosedDay = true) {
            const [markDate, markDateMore] = this.forMatArgs()
            this.dateTop = `${date.getFullYear()}年${date.getMonth() + 1}月`
            let arr = timeUtil.getMonthList(this.myDate)
            for (let i = 0; i < arr.length; i++) {
                let markClassName = ''
                let k = arr[i]
                k.chooseDay = false
                const nowTime = k.date
                const t = new Date(nowTime).getTime() / 1000
                // 看每一天的class
                for (const c of markDateMore) {
                    if (c.date === nowTime) {
                        markClassName = c.className || ''
                    }
                }
                // 标记选中某些天 设置class
                k.markClassName = markClassName
                k.isMark = markDate.indexOf(nowTime) > -1
                // 无法选中某天
                k.dayHide = t < this.agoDayHide || t > this.futureDayHide
                if (k.isToday) {
                    this.$emit('isToday', nowTime)
                }
                let flag = !k.dayHide && k.otherMonth === 'nowMonth'
                if (chooseDay && chooseDay === nowTime && flag) {
                    this.$emit('choseDay', nowTime)
                    this.$emit('click', function () {
                    })
                    this.historyChose.push(nowTime)
                    k.chooseDay = true
                } else if (
                    this.historyChose[this.historyChose.length - 1] === nowTime &&
                    !chooseDay &&
                    flag
                ) {
                    k.chooseDay = true
                }
            }
            this.list = arr
        }
    },
    mounted () {
        this.getList(this.myDate)
    },
    watch: {
        markDate: {
            handler (val, oldVal) {
                this.getList(this.myDate)
            },
            deep: true
        },
        markDateMore: {
            handler (val, oldVal) {
                this.getList(this.myDate)
            },
            deep: true
        },
        agoDayHide: {
            handler (val, oldVal) {
                this.getList(this.myDate)
            },
            deep: true
        },
        futureDayHide: {
            handler (val, oldVal) {
                this.getList(this.myDate)
            },
            deep: true
        },
        sundayStart: {
            handler (val, oldVal) {
                this.intStart()
                this.getList(this.myDate)
            },
            deep: true
        }
    }
}
</script>
