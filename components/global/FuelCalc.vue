<template>
  <div id="fuel-calc">
    <form action="/" novalidate>
      <div class="r no-gap">
        <div class="c-50">
          <h5>Track Preset</h5>
        </div>
        <div class="c-50">
          <h5>Pace</h5>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-50">
          <select id="track" name="Track" @change="selectTrack">
            <option v-for="(track, i) in tracks" :key="i" :value="track.name">
              {{ track.name }}
            </option>
          </select>
        </div>
        <div class="c-50">
          <div class="horizontal">
            <label>
              <input v-model="lapMin" class="min" max="9" min="0" name="lap_min"
                     title="Minutes" type="number"/>
              :
              <input v-model="lapSec" class="sec" max="59" min="0" name="lap_sec"
                     title="Seconds" type="number"/>
              .
              <input v-model="lapTho" class="tho" max="999" min="0" name="lap_tho"
                     title="Milliseconds" type="number"/>
            </label>
          </div>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-50">
          <h5>Fuel consumption per lap</h5>
        </div>
        <div class="c-50">
          <h5>Fuel per Stint</h5>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-50">
          <label>
            <input v-model="avgFuel" class="tho" max="9.9" min="0.0" name="fuel" step="0.1"
                   type="number">
            litres
          </label>
        </div>
        <div class="c-50">
          <label>
            <input v-model="fuelCapacity" class="tho" max="999" min="0" name="fuel-tank" step="10"
                   type="number">
            litres
          </label>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-50">
          <h5>Race Duration</h5>
        </div>
        <div class="c-50">
          <h5>Additional Out/In laps</h5>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-50">
          <label>
            <input v-model="raceHour" class="sec" max="24" min="0" name="race_hour"
                   title="Hours" type="number"/>
            h
            <input v-model="raceMin" class="sec" max="59" min="0" name="race_min"
                   title="Minutes" type="number"/>
            m
          </label>
        </div>
        <div class="c-50">
          <label>
            <input v-model="extraLaps" class="sec" max="9" min="0" name="laps"
                   title="Laps" type="number"/>
            laps
          </label>
        </div>
      </div>

      <div class="r no-gap">
        <div class="c-100">
          <h5>Results</h5>
          <div class="results">
            <span>&nbsp;#Duration&nbsp;&nbsp;<b># Fuel</b>&nbsp;&nbsp;&nbsp;&nbsp;# Laps
              &nbsp;&nbsp;&nbsp;# Stints</span>
            <ul id="result_ls">
              <li v-for="(result, i) in results" :key="i">
                &nbsp;&nbsp;
                <template v-if="result.h === '00'">&nbsp;&nbsp;&nbsp;</template>
                <template v-else>{{ result.h }}h</template>
                {{ result.mins }}m&nbsp;&nbsp;
                <b>{{ result.litres }} l</b>&nbsp;&nbsp;
                <i>{{ result.laps }}</i> laps
                in {{ result.stints }} stints
                <template v-if="i === 0">
                  <p></p>
                </template>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "FuelCalc",
  data: function () {
    return {
      lapMin: 1,
      lapSec: 45,
      lapTho: 345,
      avgFuel: 3.4,
      fuelCapacity: 120,
      raceHour: 1,
      raceMin: 30,
      extraLaps: 0,
      tracks: [
        {name: 'Barcelona', m: 1, s: 45, t: 150, fuel: 3.2},
        {name: 'Brands Hatch', m: 1, s: 24, t: 250, fuel: 2.9}
      ],
      resultDurationPresets: [60, 45, 30, 25, 20, 15, 10, 5],
    }
  },
  methods: {
    calculateFuel: function (duration) {
      /* Simple fuel estimation by number of laps for given fuel consumption per lap */
      const laps = Math.ceil((duration * 60) / this.lapTime) + Number(this.extraLaps)
      const fuelAmount = Math.round(laps * this.avgFuel)
      const stints = Math.floor(fuelAmount / this.fuelCapacity) + 1
      return [fuelAmount, laps, stints]
    },
    selectTrack: function (event) {
      this.tracks.forEach(track => {
        if (track.name === event.target.value) {
          this.lapMin = track.m
          this.lapSec = track.s
          this.lapTho = track.t
          this.avgFuel = track.fuel
        }
      })
    },
    generateResults: function () {
      let results = []

      const userDuration = Number(this.raceHour) * 60 + Number(this.raceMin)

      let durations = [...this.resultDurationPresets]
      durations.splice(0, 0, userDuration)

      durations.forEach(duration => {
        const f = this.calculateFuel(duration)
        const hrs = Math.floor(duration / 60)
        const min = duration % 60
        const r = {
          h: String(hrs).padStart(2, '0'), mins: String(min).padStart(2, '0'),
          litres: String(f[0]).padStart(3, '0'), laps: String(f[1]).padStart(2, '0'),
          stints: f[2]
        }
        results.push(r)
      })

      return results
    }
  },
  computed: {
    lapTime: function () {
      const lapMin = Number(this.lapMin) * 60
      const lapSec = Number(this.lapSec)
      const lapTho = Number(this.lapTho) / 1000
      return lapMin + lapSec + lapTho
    },
    results: function () {
      return this.generateResults()
    }
  },
  created() {
  }
}
</script>

<style scoped>

input, select, label, option {
  font-size: calc(18px + 0.4vw);
}

h2 {
  font-weight: 800;
  font-size: 1.3em;
  margin: 0 0 5px 0;
}

h5 {
  font-weight: 600;
  font-size: 1.0em;
  margin: 0 0 5px 0;
}

section {
  margin: 20px 0 5px 0;
}

p.desc {
  margin: 2px 0;
  padding: 0;
  font-size: 0.8em;
}

.horizontal {
  display: inline-block;
  width: auto !important;
  margin: 2px 0;
  padding: 0;
  height: 24px;
}

#result_ls {
  list-style-type: none;
  color: #707070;
  margin: 0;
  padding: 0;
}

footer {
  font-size: 0.8em;
  margin-top: 20px;
}

footer a {
  color: #707070;
}

.results {
  font-family: Consolas, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono, Bitstream Vera Sans Mono, Courier New, monospace;
}

input, select {
  background-color: whitesmoke;
  color: black;
}
</style>
