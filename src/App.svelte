<script>
  import { getMoonTimes, getMoonIllumination } from "suncalc";
  import { onMount } from "svelte";

  class Moon {
    dailyData = $state([]);
    todaysDate = new Date();
    static dateFormatter = new Intl.DateTimeFormat("en-US", { weekday: "short",});

    constructor(coordinates) {
      this.coordinates = coordinates ?? {
        longitude: 33.9908,
        latitude: -116.0592,
      };
    }

    getMoonData(nDays) {
      const data = [];
      for (let i = 0; i < nDays; i++) {
        const date = new Date(this.todaysDate);
        date.setDate(date.getDate() + i);
        const moonTimes = getMoonTimes(
          date,
          this.coordinates.longitude,
          this.coordinates.latitude,
        );
        const moonIllumination = getMoonIllumination(date);
        data.push({ ...moonIllumination, ...moonTimes, date });
      }
      this.dailyData = data;
    }

    calculateMoonPhase(phase) {
      const phases = [
        "New Moon",
        "Waxing Crescent",
        "First Quarter",
        "Waxing Gibbous",
        "Full Moon",
        "Waning Gibbous",
        "Last Quarter",
        "Waning Crescent",
      ];

      return phases[Math.floor(phase * 8)] ?? "New Moon";
    }

    static getWeekday(date) {
      return this.dateFormatter.format(date);
    }
  }

  const moon = new Moon();

  onMount(() => {
    moon.getMoonData(15);
  });
</script>
