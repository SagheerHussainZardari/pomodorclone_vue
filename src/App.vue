<template>
  <div
    class="w-screen h-screen transition-all duration-500"
    :class="
      tab == 1 ? 'bg-[#D95550]' : tab == 2 ? 'bg-[#4c9195]' : 'bg-[#457d9f]'
    "
  >
    <!-- Header Start -->
    <div class="w-4/12 mx-auto py-4">
      <div class="flex justify-between">
        <a href="/" class="flex items-center gap-1 text-white">
          <img src="./assets/icon-white.png" class="w-5 h-5" alt="" srcset="" />
          <span class="font-bold text-lg mt-1">Pomofocuss</span></a
        >

        <div
          @click="settingsShow = !settingsShow"
          class="bg-white bg-opacity-30 flex items-center gap-1 text-sm text-gray-300 hover:text-white px-3 py-2 cursor-pointer rounded"
        >
          <img
            src="./assets/config-white.png"
            class="w-4 h-4"
            alt=""
            srcset=""
          />
          Setting
        </div>
      </div>
      <div class="border-[1px] border-opacity-20 border-gray-600 my-2"></div>
    </div>
    <!-- Header End -->

    <!-- Timer Box Start -->
    <div class="bg-white bg-opacity-10 rounded-md p-6 w-3/12 mx-auto mt-6">
      <div class="flex justify-center gap-2 w-10/12 mx-auto">
        <div
          @click="switchTab(1)"
          :class="
            tab == 1
              ? 'text-white font-bold bg-gray-900 px-3 py-1 cursor-pointer rounded-md bg-opacity-10'
              : 'text-white font-semibold px-3 py-1 rounded-md cursor-pointer bg-opacity-10'
          "
        >
          Pomodoro
        </div>
        <div
          @click="switchTab(2)"
          :class="
            tab == 2
              ? 'text-white font-bold bg-gray-900 px-3 py-1 cursor-pointer rounded-md bg-opacity-10'
              : 'text-white font-semibold px-3 py-1 rounded-md cursor-pointer bg-opacity-10'
          "
        >
          Short Break
        </div>
        <div
          @click="switchTab(3)"
          :class="
            tab == 3
              ? 'text-white font-bold bg-gray-900 px-3 py-1 cursor-pointer rounded-md bg-opacity-10'
              : 'text-white font-semibold px-3 py-1 rounded-md cursor-pointer bg-opacity-10'
          "
        >
          Long Break
        </div>
      </div>

      <div class="mt-4 text-white text-[120px] text-center font-bold">
        {{ timeShow }}
      </div>

      <div class="text-center">
        <button
          v-if="!timmerRunning"
          class="bg-white mx-auto mt-4 rounded-t-md pt-3"
        >
          <h1
            class="font-bold px-16 pb-3 text-[22px] transition-all duration-500"
            :class="
              tab == 1
                ? 'text-[#D95550]'
                : tab == 2
                ? 'text-[#4c9195]'
                : 'text-[#457d9f]'
            "
            @click="startTimer()"
          >
            START
          </h1>
          <div class="bg-gray-200 border-4 w-full"></div>
        </button>
        <button v-else class="bg-white mx-auto mt-4 rounded-t-md pt-3">
          <h1
            class="font-bold px-16 pb-3 text-[22px] transition-all duration-500"
            :class="
              tab == 1
                ? 'text-[#D95550]'
                : tab == 2
                ? 'text-[#4c9195]'
                : 'text-[#457d9f]'
            "
            @click="stopTimer()"
          >
            STOP
          </h1>
          <div class="bg-gray-200 border-4 w-full"></div>
        </button>
      </div>
    </div>
    <!-- Timer Box End -->

    <!-- Settings Section Start -->
    <div
      v-if="settingsShow"
      class="bg-gray-900 flex justify-center bg-opacity-40 p-8 fixed top-0 w-screen min-h-screen"
    >
      <div
        class="w-3/12 m-4 px-4 bg-white rounded-lg break-words p-4 h-auto inline-block"
      >
        <div class="flex justify-between">
          <div
            class="flex justify-end text-[#bbbbbb] text-lg font-bold m-2 cursor-pointer"
          >
            TIMER SETTING
          </div>
          <div
            class="flex justify-end text-gray-600 text-lg font-bold m-2 cursor-pointer"
            @click="settingsShow = !settingsShow"
          >
            <img
              src="./assets/remove-black-sm.png"
              class="w-5 h-5 opacity-30"
              alt=""
              srcset=""
            />
          </div>
        </div>
        <hr />

        <div class="p-4">
          <h1 class="text-[#555555] font-semibold">Time (minutes)</h1>
          <div class="grid grid-cols-3 gap-6">
            <div>
              <label for="" class="text-gray-400 font-semibold text-sm"
                >Pomodoro</label
              >
              <input
                type="number"
                maxlength="5"
                @change="pomodoroTimeChanged($event)"
                class="flex w-full bg-[#efefef] outline-none ring-0 border-0 rounded p-2"
                min="0"
                :value="settings.pomodoro_time"
                minlength="0"
              />
            </div>
            <div>
              <label for="" class="text-gray-400 font-semibold text-sm"
                >Short Break</label
              >
              <input
                type="number"
                maxlength="5"
                @change="shortBreakTimeChanged($event)"
                class="flex w-full bg-[#efefef] outline-none ring-0 border-0 rounded p-2"
                min="0"
                :value="settings.short_break_time"
                minlength="0"
              />
            </div>
            <div>
              <label for="" class="text-gray-400 font-semibold text-sm"
                >Long Break</label
              >
              <input
                type="number"
                maxlength="5"
                @change="longBreakTimeChanged($event)"
                :value="settings.long_break_time"
                class="flex w-full bg-[#efefef] outline-none ring-0 border-0 rounded p-2"
                min="0"
                minlength="0"
              />
            </div>
          </div>
        </div>

        <hr />

        <div class="p-4 flex justify-between items-center">
          <h1 class="text-[#555555] font-semibold">Auto start Breaks?</h1>

          <label class="cursor-pointer">
            <div class="relative">
              <!-- input -->
              <input
                type="checkbox"
                v-model="settings.auto_start_breaks"
                id="toggleA"
                @change="autoStartBreaks($event)"
                class="sr-only"
              />
              <!-- line -->
              <div
                class="block w-14 h-8 rounded-full"
                :class="
                  settings.auto_start_breaks ? 'bg-[#84c733]' : 'bg-[#cccccc]'
                "
              ></div>
              <!-- dot -->
              <div
                class="dot absolute left-1 top-1 bg-white w-6 h-6 rounded-full transition"
              ></div>
            </div>
          </label>
        </div>
        <hr />

        <div class="p-4 flex justify-between items-center">
          <h1 class="text-[#555555] font-semibold">Auto start Pomodoros?</h1>
          <!-- <input type="checkbox"  /> -->
          <label class="cursor-pointer">
            <div class="relative">
              <!-- input -->
              <input
                type="checkbox"
                v-model="settings.auto_start_pomodoros"
                id="toggleB"
                @change="autoStartPomodoros($event)"
                class="sr-only"
              />
              <!-- line -->
              <div
                class="block w-14 h-8 rounded-full"
                :class="
                  settings.auto_start_pomodoros
                    ? 'bg-[#84c733]'
                    : 'bg-[#cccccc]'
                "
              ></div>
              <!-- dot -->
              <div
                class="dot absolute left-1 top-1 bg-white w-6 h-6 rounded-full transition"
              ></div>
            </div>
          </label>
        </div>

        <hr />

        <div class="p-4 flex justify-between items-center">
          <h1 class="text-[#555555] font-semibold">Alarm Sound</h1>

          <div class="">
            <div class="dropdown inline-block min-w-24 relative z-50">
              <button
                class="bg-[#efefef] text-gray-700 py-2 px-4 rounded inline-flex items-center"
              >
                <span class="mr-1">{{
                  audio.alarm_audio == 1
                    ? "Bell"
                    : audio.alarm_audio == 2
                    ? "Bird"
                    : audio.alarm_audio == 3
                    ? "Digital"
                    : audio.alarm_audio == 4
                    ? "Kitchen"
                    : audio.alarm_audio == 5
                    ? "Wood"
                    : "None"
                }}</span>
                <svg
                  class="fill-current h-4 w-4"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                >
                  <path
                    d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                  />
                </svg>
              </button>
              <ul
                class="dropdown-menu border absolute hidden w-full rounded pb-2 text-gray-700 pt-1"
              >
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(0)"
                    >None</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(1)"
                    >Bell</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(2)"
                    >Bird</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(3)"
                    >Digital</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(4)"
                    >Kitchen</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelectAlarm(5)"
                    >Wood</a
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>

        <hr />

        <div class="p-4 flex justify-between items-center">
          <h1 class="text-[#555555] font-semibold">Ticking Sound?</h1>

          <div class="">
            <div class="dropdown inline-block min-w-24 relative">
              <button
                class="bg-[#efefef] text-gray-700 py-2 px-4 rounded inline-flex items-center"
              >
                <span class="mr-1">{{
                  audio.ticking_audio == 1
                    ? "Ticking Slow"
                    : audio.ticking_audio == 2
                    ? "Ticking Fast"
                    : "None"
                }}</span>
                <svg
                  class="fill-current h-4 w-4"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                >
                  <path
                    d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
                  />
                </svg>
              </button>
              <ul
                class="dropdown-menu border absolute hidden w-full rounded pb-2 text-gray-700 pt-1"
              >
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelect(0)"
                    >None</a
                  >
                </li>
                <li>
                  <a
                    class="rounded-t bg-white cursor-pointer hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelect(1)"
                    >Ticking Slow</a
                  >
                </li>
                <li class="">
                  <a
                    class="bg-white hover:bg-gray-200 cursor-pointer py-2 px-4 block whitespace-no-wrap"
                    @click.prevent="audioSelect(2)"
                    >Ticking Fast</a
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Settings Section End -->
  </div>
</template>

<script>
import ticking_slow from "./assets/audio/ticking-slow.mp3";
import ticking_fast from "./assets/audio/ticking-fast.mp3";
import alarm_bell from "./assets/audio/alarm-bell.mp3";
import alarm_bird from "./assets/audio/alarm-bird.mp3";
import alarm_digital from "./assets/audio/alarm-digital.mp3";
import alarm_kitchen from "./assets/audio/alarm-kitchen.mp3";
import alarm_wood from "./assets/audio/alarm-wood.mp3";

export default {
  components: {},

  data() {
    return {
      settingsShow: true,
      tab: 1,
      time: 300,
      timeShow: "00:00",
      interval: null,
      timmerRunning: false,

      settings: {
        pomodoro_time: 0,
        short_break_time: 0,
        long_break_time: 0,
        auto_start_breaks: false,
        auto_start_pomodoros: false,
      },

      audio: {
        ticking_audio: 1,
        alarm_audio: 1,
        current_audio: null,
        ticking_slow: ticking_slow,
        ticking_fast: ticking_fast,
        alarm_bell: alarm_bell,
        alarm_bird: alarm_bird,
        alarm_digital: alarm_digital,
        alarm_kitchen: alarm_kitchen,
        alarm_wood: alarm_wood,
      },
    };
  },

  mounted() {
    this.audio.current_audio = new Audio();
    this.time = localStorage.getItem("pomodoro_time_seconds") || 1500;
    this.settings.pomodoro_time = this.time / 60;

    this.settings.short_break_time =
      localStorage.getItem("short_break_time_seconds") || 300;

    this.settings.short_break_time = this.settings.short_break_time / 60;

    this.settings.long_break_time =
      localStorage.getItem("long_break_time_seconds") || 900;
    this.settings.long_break_time = this.settings.long_break_time / 60;

    this.settings.auto_start_breaks =
      localStorage.getItem("auto_start_breaks") || false;
    this.settings.auto_start_pomodoros =
      localStorage.getItem("auto_start_pomodoros") || false;

    this.settings.auto_start_breaks =
      this.settings.auto_start_breaks === "true";

    this.settings.auto_start_pomodoros =
      this.settings.auto_start_pomodoros === "true";

    this.audio.ticking_audio = localStorage.getItem("ticking_audio");
    this.audio.alarm_audio = localStorage.getItem("alarm_audio");

    this.updateTime();
  },

  methods: {
    startTimer() {
      this.timmerRunning = true;
      this.playTickingSound();
      this.interval = setInterval(() => {
        this.updateTime();
      }, 1000);
    },
    stopTimer() {
      clearInterval(this.interval);
      this.timmerRunning = false;
      this.audio.current_audio.pause();
    },
    updateTime() {
      if (this.time < 0) {
        clearInterval(this.interval);
        this.timmerRunning = false;
        this.audio.current_audio.pause();
        this.audio.current_audio.src = this.audio.alarm_bell;
        this.audio.current_audio.load();
        this.audio.current_audio.loop = false;
        this.audio.current_audio.play();

        if (this.tab == 1) {
          this.tab = 2;
          this.timeValuesUpdate();

          if (this.settings.auto_start_breaks) {
            this.startTimer();
          }
          return;
        } else {
          this.tab = 1;

          if (this.settings.auto_start_pomodoros) {
            this.startTimer();
          }
          this.timeValuesUpdate();

          return;
        }
      }
      var minutes = this.time / 60;
      var seconds = this.time % 60;

      minutes = Math.floor(minutes);
      seconds = Math.floor(seconds);

      if (minutes < 10) {
        minutes = "0" + minutes;
      }
      if (seconds < 10) {
        seconds = "0" + seconds;
      }

      this.timeShow = "" + minutes + ":" + seconds;
      this.time--;
    },

    switchTab(tab) {
      if (this.timmerRunning) {
        if (confirm("Timer is still running do you want to continue")) {
          clearInterval(this.interval);
          this.timmerRunning = false;
          this.audio.current_audio.pause();

          this.tab = tab;
          this.timeValuesUpdate();
        }
      } else {
        this.tab = tab;
        this.timeValuesUpdate();
      }
    },

    timeValuesUpdate() {
      if (this.tab == 1) {
        this.time = localStorage.getItem("pomodoro_time_seconds") || 1500;
      }
      if (this.tab == 2) {
        this.time = localStorage.getItem("short_break_time_seconds") || 300;
      }
      if (this.tab == 3) {
        this.time = localStorage.getItem("long_break_time_seconds") || 900;
      }

      this.updateTime();
    },

    pomodoroTimeChanged(event) {
      localStorage.setItem("pomodoro_time_seconds", event.target.value * 60);
      this.time = event.target.value * 60;
      this.settings.pomodoro_time = this.time / 60;

      this.updateTime();
    },

    shortBreakTimeChanged(event) {
      localStorage.setItem("short_break_time_seconds", event.target.value * 60);

      this.time = event.target.value * 60;
      this.settings.short_break_time = this.time / 60;
      this.updateTime();
    },

    longBreakTimeChanged(event) {
      localStorage.setItem("long_break_time_seconds", event.target.value * 60);

      this.time = event.target.value * 60;
      this.settings.long_break_time = this.time / 60;
      this.updateTime();
    },

    autoStartBreaks() {
      localStorage.setItem(
        "auto_start_breaks",
        this.settings.auto_start_breaks
      );
    },
    autoStartPomodoros() {
      localStorage.setItem(
        "auto_start_pomodoros",
        this.settings.auto_start_pomodoros
      );
    },

    playTickingSound() {
      if (this.audio.ticking_audio == 1 || this.audio.ticking_audio == 2) {
        this.audio.current_audio.src =
          this.audio.ticking_audio == 1
            ? this.audio.ticking_slow
            : this.audio.ticking_fast;

        this.audio.current_audio.load();
        this.audio.current_audio.loop = true;
        this.audio.current_audio.play();
      }
    },

    audioSelect(option) {
      localStorage.setItem("ticking_audio", option);
      this.audio.ticking_audio = option;

      if (option == 1 || option == 2) {
        this.audio.current_audio.src =
          this.audio.ticking_audio == 1
            ? this.audio.ticking_slow
            : this.audio.ticking_fast;

        this.audio.current_audio.load();

        this.audio.current_audio.play();

        setTimeout(() => {
          this.audio.current_audio.pause();
        }, 2000);
      }
    },

    audioSelectAlarm(option) {
      localStorage.setItem("alarm_audio", option);
      this.audio.alarm_audio = option;

      if (
        option == 1 ||
        option == 2 ||
        option == 3 ||
        option == 4 ||
        option == 5
      ) {
        this.audio.current_audio.src =
          this.audio.alarm_audio == 1
            ? this.audio.alarm_bell
            : this.audio.alarm_audio == 2
            ? this.audio.alarm_bird
            : this.audio.alarm_audio == 3
            ? this.audio.alarm_digital
            : this.audio.alarm_audio == 4
            ? this.audio.alarm_kitchen
            : this.audio.alarm_audio == 5
            ? this.audio.alarm_wood
            : "None";
        this.audio.current_audio.load();

        this.audio.current_audio.play();

        setTimeout(() => {
          this.audio.current_audio.pause();
        }, 2000);
      }
    },
  },
};
</script>

<style>
/* 'ArialRounded', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, Helvetica, Arial, sans-serif,
    'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol' */

@font-face {
  font-family: "ArialRounded";
  src: local("ArialRounded"),
    url(./assets/fonts/Arial_Rounded_MT.ttf) format("truetype");
}

* {
  font-family: "ArialRounded" !important;
}

input:checked ~ .dot {
  transform: translateX(100%);
  background-color: #ffffff;
}

.dropdown:hover .dropdown-menu {
  display: block;
}
</style>
