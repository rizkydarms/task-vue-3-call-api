<template>
  <v-container>
    <!-- Daftar Akun -->
    <v-card>
      <v-card-title>Daftar Akun</v-card-title>
      <v-card-text>
        <v-list>
          <v-list-item v-for="account in accounts" :key="account.id">
            <v-list-item-content>
              <v-list-item-title>{{ account.account_id }}</v-list-item-title>
              <v-list-item-subtitle>{{ account.name }}</v-list-item-subtitle>
              <v-list-item-subtitle>
                <v-chip color="green" text-color="white">Saldo: Rp.{{ account.balance }}</v-chip>
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card-text>
    </v-card>

    <!-- Ringkasan Akun -->
    <v-card class="mt-4">
      <v-card-title>Ringkasan</v-card-title>
      <v-card-text>
        <v-row>
          <v-col cols="12" md="4">
            <p><strong>Total Akun:</strong> {{ totalAccounts }}</p>
          </v-col>
          <v-col cols="12" md="4">
            <p><strong>Total Saldo:</strong> Rp {{ totalBalance.toLocaleString() }}</p>
          </v-col>
          <v-col cols="12" md="4">
            <p><strong>Rata-rata Saldo:</strong> Rp {{ averageBalance.toFixed(2) }}</p>
          </v-col>
        </v-row>

        <!-- Progress Bar untuk Total Saldo -->
        <v-progress-linear
          :value="totalBalancePercentage"
          height="10"
          color="blue"
          class="mt-4"
        >
          <span>{{ totalBalance.toLocaleString() }}</span>
        </v-progress-linear>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import api from '@/plugins/api';

export default {
  data() {
    return {
      accounts: [],
      totalAccounts: 0,
      totalBalance: 0,
      averageBalance: 0
    };
  },
  created() {
    this.fetchAccounts();
  },
  methods: {
  fetchAccounts() {
    api.get('/account/list')
      .then(response => {
        this.accounts = response.data;
        // this.calculateStatistics(response.data);
      })
      .catch(error => {
        console.error("Error fetching account list:", error);
      });
  }
},

  computed: {
    totalBalancePercentage() {
      // Asumsikan total maksimum saldo bisa 10 juta, bisa diganti dengan logika lainnya
      const maxBalance = 10000000; // Gantilah sesuai dengan total saldo maksimum yang Anda inginkan
      return Math.min((this.totalBalance / maxBalance) * 100, 100); // Membatasi 100% sebagai nilai maksimum
    }
  }
};
</script>

<style scoped>
.mt-4 {
  margin-top: 16px;
}
</style>
