<template>
  <div>
    <h1>Your Payments</h1>
    <ul>
      <li v-for="payment in payments" :key="payment.id">
        {{ payment.description }} - {{ payment.amount }}
        <button @click="openModal(payment.id)">Locate</button>
      </li>
    </ul>
    <location-modal v-if="showModal" :paymentId="selectedPaymentId" @close="closeModal" />
  </div>
</template>

<script>
import LocationModal from '@/components/LocationModal.vue';

export default {
  components: {
    LocationModal,
  },
  data() {
    return {
      payments: [],
      showModal: false,
      selectedPaymentId: null,
    };
  },
  methods: {
    openModal(paymentId) {
      this.selectedPaymentId = paymentId;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
  },
  async created() {
    try {
      const response = await this.$axios.get('/transactions');
      this.payments = response.data;
    } catch (error) {
      console.error('Failed to fetch payments', error);
    }
  },
};
</script>
