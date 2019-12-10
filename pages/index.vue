<template>
  <el-main>
    <h1>
      Nuxt.js Advent Calendar 2019
    </h1>

    <p>{{ comment }}</p>

    <el-table
      :data="date"
      border
    >
      <el-table-column
        v-for="index in [0, 1, 2, 3, 4, 5, 6]"
        :key="index"
        :prop="dateKeys[index]"
        :label="dayNames[index]"
      >
        <template slot-scope="scope">
          <div v-if="days[scope.row[dateKeys[index]]]">
            <el-tag>{{ days[scope.row[dateKeys[index]]].date }}</el-tag>

            <el-row type="flex" justify="start" align="middle">
              <el-col>
                <el-avatar
                  shape="square"
                  :size="50"
                  :src="days[scope.row[dateKeys[index]]].authorImageUrl"
                  :alt="days[scope.row[dateKeys[index]]].authorName"
                ></el-avatar>
              </el-col>

              <el-col>
                <el-link
                  type="primary"
                  :href="`https://qiita.com/${days[scope.row[dateKeys[index]]]}`"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="author-name"
                >
                  {{ days[scope.row[dateKeys[index]]].authorName }}
                </el-link>
              </el-col>
            </el-row>

            <el-link v-if="days[scope.row[dateKeys[index]]].articleUrl !== null"
              type="primary"
              :href="days[scope.row[dateKeys[index]]].articleUrl"
              target="_blank"
              rel="noopener noreferrer"
            >
              {{ days[scope.row[dateKeys[index]]].comment }}
            </el-link>

            <p v-else>
              {{ days[scope.row[dateKeys[index]]].comment }}
            </p>
          </div>

          <div v-else>
            <el-tag type="info">
              {{ (scope.row[dateKeys[index]] + 1) }}
            </el-tag>
          </div>
        </template>
      </el-table-column>
    </el-table>

    <p>※ このページはレスポンシブ対応していません。デスクトップサイズの画面でご覧ください。</p>
  </el-main>
</template>

<script>
import { parse } from 'node-html-parser'
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('https://qiita.com/advent-calendar/2019/nuxt-js')
    const root = parse(data)

    return {
      comment: root.querySelector('.markdownContent').text,
      dayNames: root.querySelectorAll('.adventCalendarCalendar_dayName').map(dayName => dayName.text),
      days: root.querySelectorAll('.adventCalendarCalendar_day').map(day => {
        const articleUrl = day.querySelector('.adventCalendarCalendar_comment').querySelector('a') ?
          day.querySelector('.adventCalendarCalendar_comment').querySelector('a').attributes['href'] :
          null
        return {
          date: day.querySelector('.adventCalendarCalendar_date').text,
          authorName: day.querySelector('.adventCalendarCalendar_author').text.replace(/\s|&nbsp;/g, ''),
          authorImageUrl: day.querySelector('.adventCalendarCalendar_authorIcon').attributes['src'],
          comment: day.querySelector('.adventCalendarCalendar_comment').text,
          articleUrl: articleUrl
        }
      })
    }
  },
  data() {
    return {
      date: [0, 1, 2, 3].map(weekNumber => {
        return {
          sun: 0 + (7 * weekNumber),
          mon: 1 + (7 * weekNumber),
          tue: 2 + (7 * weekNumber),
          wed: 3 + (7 * weekNumber),
          thu: 4 + (7 * weekNumber),
          fri: 5 + (7 * weekNumber),
          sat: 6 + (7 * weekNumber)
        }
      }),
      dateKeys: ['sun', 'mon', 'tue', 'wed', 'thu', 'fri', 'sat']
    }
  }
}
</script>

<style>
.el-table__row .el-tag {
  display: block;
  width: 100%;
}
.el-table__row .el-avatar {
  display: inline-block;
  margin: 10px 10px 10px 0;
}
.el-table__row .author-name {
  font-size: 13px;
}
.el-table__row .author-name .el-link--inner {
  width: auto;
  max-width: 80px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.el-table td {
  vertical-align: top;
}
</style>
