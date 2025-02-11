<!-- App.vue -->
<template>
  <div class="portfolio-app">
    <!-- Navigation Component -->
    <nav class="nav-bar">
      <div class="nav-items">
        <span 
          v-for="section in sections" 
          :key="section.id"
          @click="currentSection = section.id"
          :class="{ active: currentSection === section.id }"
          class="nav-item"
        >
          {{ section.name }}
        </span>
      </div>
    </nav>

    <!-- Personal Info Section -->
    <section v-if="currentSection === 'personal'" class="personal-section">
      <div class="profile-container">
        <img :src="personalInfo.profileImage" alt="Profile Picture" class="profile-image">
        <h1>{{ personalInfo.name }}</h1>
        <h2>{{ personalInfo.title }}</h2>
        <p>{{ personalInfo.bio }}</p>
      </div>
    </section>

    <!-- Projects Section -->
    <section v-else-if="currentSection === 'projects'" class="projects-section">
      <div class="filters">
        <select v-model="selectedTechnology" @change="filterProjects">
          <option value="">Todas las tecnologías</option>
          <option v-for="tech in technologies" :key="tech" :value="tech">
            {{ tech }}
          </option>
        </select>
      </div>

      <div class="projects-grid">
        <div 
          v-for="project in filteredProjects" 
          :key="project.id"
          class="project-card"
        >
          <img :src="project.image" :alt="project.name" class="project-image">
          <h3>{{ project.name }}</h3>
          <p>{{ project.description }}</p>
          <div class="tech-stack">
            <span v-for="tech in project.technologies" :key="tech" class="tech-tag">
              {{ tech }}
            </span>
          </div>
          <div class="project-links">
            <a :href="project.github" target="_blank" class="link-button">GitHub</a>
            <a :href="project.demo" target="_blank" class="link-button">Demo</a>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section v-else class="contact-section">
      <form @submit.prevent="submitForm" class="contact-form">
        <div class="form-group">
          <label for="name">Nombre:</label>
          <input 
            type="text" 
            id="name" 
            v-model="contactForm.name" 
            required
          >
        </div>
        <div class="form-group">
          <label for="email">Email:</label>
          <input 
            type="email" 
            id="email" 
            v-model="contactForm.email" 
            required
          >
        </div>
        <div class="form-group">
          <label for="message">Mensaje:</label>
          <textarea 
            id="message" 
            v-model="contactForm.message" 
            required
          ></textarea>
        </div>
        <button type="submit" :disabled="sending">
          {{ sending ? 'Enviando...' : 'Enviar' }}
        </button>
      </form>

      <div class="social-links">
        <a 
          v-for="link in socialLinks" 
          :key="link.name"
          :href="link.url"
          target="_blank"
          class="social-link"
        >
          {{ link.name }}
        </a>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'PortfolioApp',
  data() {
    return {
      currentSection: 'personal',
      selectedTechnology: '',
      sending: false,
      sections: [
        { id: 'personal', name: 'Sobre mí' },
        { id: 'projects', name: 'Proyectos' },
        { id: 'contact', name: 'Contacto' }
      ],
      personalInfo: {
        name: 'Ricardo Carrillo Cruz',
        title: 'Desarrollador Full Stack',
        bio: 'Desarrollador web con experiencia en Vue.js, Node.js y MongoDB. Apasionada por crear experiencias web únicas y funcionales.',
        profileImage: require('@/assets/profile.jpg')
      },
      projects: [
        {
          id: 1,
          name: 'E-commerce Platform',
          description: 'Plataforma de comercio electrónico completa con carrito de compras y pasarela de pago',
          technologies: ['Vue.js', 'Node.js', 'MongoDB'],
          image: require('@/assets/ecommerce.png'),
          github: 'https://github.com/username/ecommerce',
          demo: 'https://demo-ecommerce.com'
        },
        {
          id: 2,
          name: 'Task Manager',
          description: 'Aplicación de gestión de tareas con características de colaboración en tiempo real',
          technologies: ['Vue.js', 'Firebase', 'Tailwind CSS'],
          image: require('@/assets/taskmanager.jpg'),
          github: 'https://github.com/username/taskmanager',
          demo: 'https://demo-taskmanager.com'
        }
      ],
      contactForm: {
        name: '',
        email: '',
        message: ''
      },
      socialLinks: [
        { name: 'LinkedIn', url: 'https://linkedin.com/in/ricardo-carrillo-cruz-6b78997' },
        { name: 'GitHub', url: 'https://github.com/rcarrillocruz' }
      ]
    }
  },
  computed: {
    technologies() {
      return [...new Set(this.projects.flatMap(p => p.technologies))]
    },
    filteredProjects() {
      if (!this.selectedTechnology) return this.projects
      return this.projects.filter(p => 
        p.technologies.includes(this.selectedTechnology)
      )
    }
  },
  methods: {
    async submitForm() {
      this.sending = true
      try {
        // Simulación de envío de formulario
        await new Promise(resolve => setTimeout(resolve, 1000))
        alert('Mensaje enviado correctamente')
        this.contactForm = { name: '', email: '', message: '' }
      } catch (error) {
        alert('Error al enviar el mensaje')
      } finally {
        this.sending = false
      }
    },
    filterProjects() {
      // Método adicional para posible expansión de la funcionalidad de filtrado
    }
  }
}
</script>

<style>
.portfolio-app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

.nav-bar {
  background-color: #2c3e50;
  padding: 15px;
  margin-bottom: 30px;
  border-radius: 8px;
}

.nav-items {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.nav-item {
  color: white;
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.nav-item:hover {
  background-color: #34495e;
}

.nav-item.active {
  background-color: #3498db;
}

.profile-container {
  text-align: center;
  margin-bottom: 40px;
}

.profile-image {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  margin-bottom: 20px;
  object-fit: cover;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
  margin-top: 30px;
}

.project-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.project-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 4px;
  margin-bottom: 15px;
}

.tech-stack {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 15px 0;
}

.tech-tag {
  background-color: #e0e0e0;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.9em;
}

.project-links {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.link-button {
  background-color: #3498db;
  color: white;
  padding: 8px 16px;
  border-radius: 4px;
  text-decoration: none;
  transition: background-color 0.3s;
}

.link-button:hover {
  background-color: #2980b9;
}

.contact-form {
  max-width: 600px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.form-group textarea {
  height: 150px;
}

button {
  background-color: #2ecc71;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #27ae60;
}

button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.social-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 30px;
}

.social-link {
  color: #3498db;
  text-decoration: none;
  font-weight: bold;
}

.social-link:hover {
  text-decoration: underline;
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
  
  .nav-items {
    flex-direction: column;
    align-items: center;
  }
  
  .nav-item {
    width: 100%;
    text-align: center;
  }
}
</style>
