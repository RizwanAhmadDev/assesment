<template>
    <div class="app-container">
      <!-- Sidebar -->
      <aside class="sidebar">
        <nav>
          <ul>
            <li><a href="#">Dashboard</a></li>
            <li><a href="#">Emails</a></li>
            <li><a href="#">Time Usage</a></li>
            <li><a href="#">Invoices</a></li>
            <li><a href="#">Settings</a></li>
          </ul>
        </nav>
      </aside>
  
      <!-- Main Content -->
      <main class="content">
        <!-- Form Section -->
        <section class="form-section">
          <form @submit.prevent="submitForm" class="email-form">
            <div class="form-group">
              <label for="title">Title:</label>
              <input type="text" v-model="title" id="title" required />
            </div>
  
            <div class="form-group">
              <label for="appointee">Appointee:</label>
              <input type="text" v-model="appointee" id="appointee" required />
            </div>
  
            <div class="form-group">
              <label for="status">Status:</label>
              <input type="text" v-model="status" id="status" required />
            </div>
  
            <div class="form-group">
              <label for="last_email">Last Email:</label>
              <input type="date" v-model="last_email" id="last_email" required />
            </div>
  
            <button type="submit" :disabled="loading" class="submit-btn">
              Submit
            </button>
          </form>
  
          <!-- Form submission feedback -->
          <p v-if="formSuccess" class="success-message">Form submitted successfully!</p>
          <p v-if="formError" class="error-message">{{ formError }}</p>
        </section>
  
        <!-- Email List Section -->
        <section class="email-list-section">
          <h3>Email List:</h3>
          <table class="email-table">
            <thead>
              <tr>
                <th>Title</th>
                <th>Appointee</th>
                <th>Status</th>
                <th>Last Email</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="email in emails" :key="email.id">
                <td>{{ email.title }}</td>
                <td>{{ email.appointee }}</td>
                <td>{{ email.status }}</td>
                <td>{{ formatDate(email.last_email) }}</td>
              </tr>
            </tbody>
          </table>
        </section>
      </main>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        title: '',
        appointee: '',
        status: '',
        last_email: '',
        emails: [],
        loading: false,
        formSuccess: false,
        formError: '',
      };
    },
    mounted() {
      this.fetchEmails();
    },
    methods: {
      async submitForm() {
        this.formError = '';
        this.formSuccess = false;
        this.loading = true;
  
        try {
          const response = await axios.post('http://localhost:8000/api/emails', {
            title: this.title,
            appointee: this.appointee,
            status: this.status,
            last_email: this.last_email,
          });
          this.emails.push(response.data);
          this.formSuccess = true;
          this.clearForm();
        } catch (error) {
          this.formError = 'An error occurred while submitting the form.';
        } finally {
          this.loading = false;
        }
      },
      async fetchEmails() {
        this.loading = true;
        try {
          const response = await axios.get('http://localhost:8000/api/emails');
          this.emails = response.data;
        } catch (error) {
          this.formError = 'Failed to load emails. Please try again later.';
        } finally {
          this.loading = false;
        }
      },
      clearForm() {
        this.title = '';
        this.appointee = '';
        this.status = '';
        this.last_email = '';
      },
      formatDate(date) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' };
        return new Date(date).toLocaleDateString(undefined, options);
      },
    },
  };
  </script>
  
  <style scoped>
  /* General Layout */
  .app-container {
    display: flex;
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    height: 100vh;
  }
  
  .sidebar {
    background-color: #2f3b52;
    width: 220px;
    padding: 20px;
    color: white;
  }
  
  .sidebar nav ul {
    list-style-type: none;
    padding: 0;
  }
  
  .sidebar nav ul li {
    margin: 20px 0;
  }
  
  .sidebar nav ul li a {
    color: white;
    text-decoration: none;
    font-size: 1rem;
  }
  
  .sidebar nav ul li a:hover {
    text-decoration: underline;
  }
  
  .content {
    flex-grow: 1;
    padding: 20px;
  }
  
  /* Form Section */
  .form-section {
    margin-bottom: 20px;
  }
  
  .email-form {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  .form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
  }
  
  .form-group input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .submit-btn {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px 15px;
    font-size: 1rem;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .submit-btn:disabled {
    background-color: #ccc;
  }
  
  .success-message {
    color: green;
    margin-top: 10px;
  }
  
  .error-message {
    color: red;
    margin-top: 10px;
  }
  
  /* Email List Section */
  .email-list-section {
    margin-top: 20px;
  }
  
  .email-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
  }
  
  .email-table th,
  .email-table td {
    padding: 10px;
    border: 1px solid #ccc;
    text-align: left;
  }
  
  .email-table th {
    background-color: #f1f1f1;
  }
  
  /* Responsive Design */
  @media (max-width: 768px) {
    .app-container {
      flex-direction: column;
    }
  
    .sidebar {
      width: 100%;
      padding: 10px;
    }
  
    .content {
      padding: 10px;
    }
  }
  </style>
  