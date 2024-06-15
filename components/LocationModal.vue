<template>
  <div class="modal">
    <div class="modal-content">
      <span class="close" @click="$emit('close')">&times;</span>
      <h2>Select Location</h2>
      <ul>
        <li v-for="place in places" :key="place.id">
          {{ place.name }}
          <button @click="selectLocation(place)">Select</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ['paymentId'],
  data() {
    return {
      places: [],
    };
  },
  methods: {
    async selectLocation(place) {
      try {
        await this.$axios.post(`/transactions/${this.paymentId}/location`, place);
        this.$emit('close');
      } catch (error) {
        console.error('Failed to update location', error);
      }
    },
  },
  async mounted() {
    try {
      const payment = await this.$axios.get(`/transactions/${this.paymentId}`);
      const { latitude, longitude } = payment.data;
      const response = await this.$axios.get(`/places?latitude=${latitude}&longitude=${longitude}`);
      this.places = response.data.results;
    } catch (error) {
      console.error('Failed to fetch places', error);
    }
  },
};
</script>
