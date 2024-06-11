<template>
  <div class="character-card">
    <img :src="character.image" alt="character image" />
    <div class="info-per">
      <div class="name-stat">
        <h2 style="font-size: 27px;">{{ character.name }}</h2>
        <div class="info-status">
          <div id="status-click" :class="statusClass"></div>
          <p style="font-size:16px">{{ character.status }} - {{ character.species }}</p>
        </div>
      </div>
      <div class="info-location">
        <p style="font-size:16px" class="style-location">Last known location:</p>
        <p style="font-size:18px">{{ character.location.name }}</p>
      </div>
      <div class="info-location">
        <p style="font-size:16px" class="style-location">First seen in:</p>
        <p style="font-size:18px">{{ firstEpisodeName }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CharacterCard',
  props: {
    character: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      firstEpisodeName: '',
    };
  },
  computed: {
    statusClass() {
      return {
        'status-alive': this.character.status === 'Alive',
        'status-dead': this.character.status === 'Dead',
        'status-unknown': this.character.status === 'unknown',
      };
    },
  },
  async mounted() {
    if (this.character.episode.length > 0) {
      const firstEpisodeUrl = this.character.episode[0];
      try {
        const response = await fetch(firstEpisodeUrl);
        const data = await response.json();
        this.firstEpisodeName = data.name;
      } catch (error) {
        console.error('Failed to fetch episode data:', error);
      }
    }
  },
};
</script>

<style scoped>
.info-location {
  font-family: -apple-system, "system-ui", "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}
p.style-location {
  color: #b3b3b3;
}
.info-location {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-items: flex-start;
}
.info-status {
  display: flex;
  align-items: center;
  justify-content: flex-start;
}
.status-alive {
  background: #55CB44;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px;
}
.status-dead {
  background: #D53C2E;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px;
}
.status-unknown {
  background: gray;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px;
}
.character-card {
  text-align: center;
  width: 600px;
  height: 220px;
  display: flex;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px, rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
}
.info-per[data-v-51ed5d59] {
    position: relative;
    color: rgb(255, 255, 255);
    display: flex;
    flex-direction: column;
    flex: 3 1 0%;
    padding: 0.75rem;
    align-content: center;
    align-items: flex-start;
    justify-content: space-around;
}
img {
  max-width: 100%;
  height: auto;
}
</style>
