<style>
.ini.countdown {
  list-style: none;
  padding: 0;
  margin: 0 -5px;
}
.ini.countdown li {
  display: inline-block;
  background: black;
  margin: 0 5px;
  min-width: 3rem;
  color: white;
  padding: 5px 2px;
  text-align: center;
}
</style>
<template>
  <div>
    <ul class="ini countdown">
      <li>
        <div class="digit">{{ setDigit(days) }}</div>
        <div class="time">{{t_dates ? t_dates:'Days'}}</div>
      </li>
      <li>
        <div class="digit">{{ setDigit(hours) }}</div>
        <div class="time">{{t_hours ? t_hours:'Hours'}}</div>
      </li>
      <li>
        <div class="digit">{{ setDigit(minutes) }}</div>
        <div class="time">{{t_minutes ? t_minutes:'Minutes'}}</div>
      </li>
      <li>
        <div class="digit">{{ setDigit(seconds) }}</div>
        <div class="time">{{t_seconds ? t_seconds:'Seconds'}}</div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  props: {
    start: Number,
    end: Number,
    t_dates: String,
    t_hours: String,
    t_minutes: String,
    t_seconds: String
  },
  data() {
    return {
      tmp_start: null,
      diff: null,
      interval: null,
      second: 0
    };
  },
  created() {
    this.interval = setInterval(() => {
      if (!this.start) {
        this.tmp_start = Math.trunc(new Date().getTime() / 1000);
      } else {
        this.tmp_start = this.start + this.second;
      }
      this.diff = this.end - this.tmp_start;
      this.second++;
    }, 1000);
  },
  computed: {
    seconds() {
      return Math.trunc(this.diff) % 60;
    },
    minutes() {
      return Math.trunc(this.diff / 60) % 60;
    },
    hours() {
      return Math.trunc(this.diff / 60 / 60) % 24;
    },
    days() {
      return Math.trunc(this.diff / 60 / 60 / 24);
    }
  },
  watch: {
    diff(val) {
      if (val <= 0) {
        this.diff = 0;
        clearInterval(this.interval);
      }
    }
  },
  methods: {
    setDigit(val) {
      if (val.toString().length <= 1) {
        return "0" + val.toString();
      }
      return val.toString();
    }
  }
};
</script>