<template>
  <div class="job-list">
    <ul>
      <li v-for="job in apiResponse" :key="job.id" class="job-item">
        <h2 class="job-title">{{ job.title }}</h2>
        <template v-if="job.crushes">
          <p><strong>Crushes:</strong> {{ job.crushes }}</p>
        </template>
        <template v-if="job.answer1">
          <p><strong>Description :</strong></p>
          <p v-html="job.answer1" class="job-description"></p>
        </template>
        <template v-if="job.answer2">
          <p class="job-answer">{{ job.answer2 }}</p>
        </template>
        <template v-if="job.answer3">
          <p class="job-answer">{{ job.answer3 }}</p>
        </template>
        <template v-if="job.type">
          <p><strong>Type de contrat:</strong> {{ job.type }}</p>
        </template>
        <template v-if="job.contactEmail">
          <p><strong>Email de contact:</strong> {{ job.contactEmail }}</p>
        </template>
        <template v-if="job.expiresAt">
          <p><strong>Expire le:</strong> {{ job.expiresAt }}</p>
        </template>
        <template v-if="job.employer">
          <p><strong>Employeur:</strong> {{ job.employer.name }}</p>
        </template>
        <template v-if="job.sectors && job.sectors.length > 0">
          <p><strong>Secteurs:</strong> {{ job.sectors.map(sector => sector.name).join(', ') }}</p>
        </template>
        <template v-else>
          <p><strong>Secteurs:</strong> Non défini</p>
        </template>
        <template v-if="job.locations && job.locations.length > 0">
          <p><strong>Emplacement(s):</strong> {{ job.locations.map(location => location.city).join(', ') }}</p>
        </template>
        <template v-else>
          <p><strong>Emplacement:</strong> Non défini</p>
        </template>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.job-list {
  max-width: 600px;
  margin: 0 auto;
  font-family: sans-serif;
}

.job-item {
  border: 1px solid #ccc;
  margin-bottom: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  list-style-type: none;
}

.job-title {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

.job-description {
  font-size: 1rem;
  line-height: 1.4;
}

.job-answer {
  font-size: 1rem;
}

strong {
  font-weight: bold;
}
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiResponse: null,
    };
  },
  mounted() {
    const params = {
      populate: ['type', 'employer', 'sectors', 'locations'],
    };

    axios.get('https://app.staging.profilpublic.fr/api/jobs', { params })
      .then(response => {
        this.apiResponse = response.data.data;
        console.log('Réponse de l\'API :', this.apiResponse);
      })
      .catch(error => {
        console.error('Erreur lors de l\'appel API :', error);
      });
  },
};
</script>