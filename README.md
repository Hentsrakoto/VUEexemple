# VUEexemple
```
// Structure simplifiée en MVC avec Vue.js frontend + json-server backend

// === BACKEND (json-server) ===
// db.json
{
  "etudiants": [],
  "professeurs": [],
  "encadrements": [],
  "soutenances": [],
  "suivi": [],
  "edt": []
}

// Commande pour lancer :
// json-server --watch backend/db.json --port 3001


// === FRONTEND Vue.js (MVC structuré) ===

// src/models/EtudiantModel.js
import axios from 'axios'
const API = 'http://localhost:3001/etudiants'

export default {
  getAll() {
    return axios.get(API)
  },
  get(id) {
    return axios.get(`${API}/${id}`)
  },
  create(data) {
    return axios.post(API, data)
  },
  update(id, data) {
    return axios.put(`${API}/${id}`, data)
  },
  delete(id) {
    return axios.delete(`${API}/${id}`)
  }
}

// src/controllers/EtudiantController.js
import EtudiantModel from '../models/EtudiantModel'

export default {
  async listEtudiants() {
    const res = await EtudiantModel.getAll()
    return res.data
  },
  async createEtudiant(etudiant) {
    await EtudiantModel.create(etudiant)
  },
  async deleteEtudiant(id) {
    await EtudiantModel.delete(id)
  }
  // etc.
}

// src/views/EtudiantsView.vue
<template>
  <div>
    <h2>Liste des étudiants</h2>
    <form @submit.prevent="addEtudiant">
      <input v-model="newEtudiant.nom" placeholder="Nom" required />
      <input v-model="newEtudiant.prenom" placeholder="Prénom" required />
      <input v-model="newEtudiant.sujet" placeholder="Sujet" required />
      <button type="submit">Ajouter</button>
    </form>

    <ul>
      <li v-for="e in etudiants" :key="e.id">
        {{ e.nom }} {{ e.prenom }} - {{ e.sujet }}
        <button @click="removeEtudiant(e.id)">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script>
import EtudiantController from '../controllers/EtudiantController'

export default {
  data() {
    return {
      etudiants: [],
      newEtudiant: { nom: '', prenom: '', sujet: '' }
    }
  },
  async mounted() {
    this.etudiants = await EtudiantController.listEtudiants()
  },
  methods: {
    async addEtudiant() {
      await EtudiantController.createEtudiant(this.newEtudiant)
      this.etudiants = await EtudiantController.listEtudiants()
      this.newEtudiant = { nom: '', prenom: '', sujet: '' }
    },
    async removeEtudiant(id) {
      await EtudiantController.deleteEtudiant(id)
      this.etudiants = await EtudiantController.listEtudiants()
    }
  }
}
</script>


```
