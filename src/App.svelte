<script>
  import { getMoonTimes, getMoonIllumination } from "suncalc";
  import { onMount } from "svelte";

  class Moon {
    dailyData = $state([]);
    todaysDate = new Date();

    constructor(coordinates) {
      this.coordinates = coordinates ?? {
        longitude: 33.9908,
        latitude: -116.0592,
      };
    }

    getMoonData(nDays) {
      const data = [];
      for (let i = 0; i < nDays; i++) {
        const currentDate = new Date(this.todaysDate);
        currentDate.setDate(currentDate.getDate() + i);
        const moonTimes = getMoonTimes(
          currentDate,
          this.coordinates.longitude,
          this.coordinates.latitude,
        );
        const moonIllumination = getMoonIllumination(currentDate);
        data.push({ ...moonIllumination, ...moonTimes, currentDate });
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
  }

  const moon = new Moon();

  onMount(() => {
    moon.getMoonData(15);
  });
</script>
