<template>
  <div class="container py-5">

    <!-- Header -->
    <div class="text-center mb-5">
      <h1 class="display-5 fw-bold text-primary">
        🏢 Employee Management System
      </h1>
      <p class="text-muted">Manage employee records with ease — Add, View, Edit, Delete</p>
    </div>

    <!-- Alert Messages -->
    <div v-if="alert.message" :class="`alert alert-${alert.type} alert-dismissible fade show`" role="alert">
      {{ alert.message }}
      <button type="button" class="btn-close" @click="alert.message = ''"></button>
    </div>

    <!-- ADD / EDIT FORM CARD -->
    <div class="card shadow-sm mb-5">
      <div class="card-header bg-primary text-white">
        <h5 class="mb-0">{{ isEditing ? '✏️ Edit Employee' : '➕ Add New Employee' }}</h5>
      </div>
      <div class="card-body">
        <div class="row g-3">

          <div class="col-md-6">
            <label class="form-label fw-semibold">Full Name</label>
            <input
              v-model="form.name"
              type="text"
              class="form-control"
              placeholder="e.g. Ravi Kumar"
            />
          </div>

          <div class="col-md-6">
            <label class="form-label fw-semibold">Designation</label>
            <input
              v-model="form.designation"
              type="text"
              class="form-control"
              placeholder="e.g. Software Engineer"
            />
          </div>

          <div class="col-md-6">
            <label class="form-label fw-semibold">Department</label>
            <select v-model="form.department" class="form-select">
              <option value="">-- Select Department --</option>
              <option>Engineering</option>
              <option>HR</option>
              <option>Finance</option>
              <option>Marketing</option>
              <option>Operations</option>
            </select>
          </div>

          <div class="col-md-6">
            <label class="form-label fw-semibold">Salary (₹)</label>
            <input
              v-model.number="form.salary"
              type="number"
              class="form-control"
              placeholder="e.g. 50000"
            />
          </div>

        </div>

        <div class="mt-4 d-flex gap-2">
          <button class="btn btn-primary px-4" @click="submitForm">
            {{ isEditing ? '💾 Update Employee' : '✅ Add Employee' }}
          </button>
          <button v-if="isEditing" class="btn btn-secondary" @click="cancelEdit">
            ✖ Cancel
          </button>
        </div>
      </div>
    </div>

    <!-- EMPLOYEE TABLE CARD -->
    <div class="card shadow-sm">
      <div class="card-header bg-dark text-white d-flex justify-content-between align-items-center">
        <h5 class="mb-0">📋 Employee Records</h5>
        <span class="badge bg-light text-dark fs-6">Total: {{ employees.length }}</span>
      </div>
      <div class="card-body p-0">

        <!-- Loading Spinner -->
        <div v-if="loading" class="text-center py-5">
          <div class="spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          <p class="mt-2 text-muted">Fetching employee data...</p>
        </div>

        <!-- Empty State -->
        <div v-else-if="employees.length === 0" class="text-center py-5">
          <p class="fs-5 text-muted">No employees found. Add one above! 👆</p>
        </div>

        <!-- Table -->
        <div v-else class="table-responsive">
          <table class="table table-hover mb-0">
            <thead class="table-light">
              <tr>
                <th>#</th>
                <th>Employee ID</th>
                <th>Name</th>
                <th>Designation</th>
                <th>Department</th>
                <th>Salary (₹)</th>
                <th class="text-center">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(emp, index) in employees" :key="emp.id">
                <td>{{ index + 1 }}</td>
                <td><span class="badge bg-secondary">{{ emp.id }}</span></td>
                <td class="fw-semibold">{{ emp.name }}</td>
                <td>{{ emp.designation }}</td>
                <td>
                  <span class="badge bg-info text-dark">{{ emp.department }}</span>
                </td>
                <td class="text-success fw-semibold">₹{{ Number(emp.salary).toLocaleString('en-IN') }}</td>
                <td class="text-center">
                  <button class="btn btn-sm btn-warning me-2" @click="editEmployee(emp)">
                    ✏️ Edit
                  </button>
                  <button class="btn btn-sm btn-danger" @click="deleteEmployee(emp.id)">
                    🗑️ Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';

// ⚠️ REPLACE THIS WITH YOUR MOCKAPI URL
const API_URL = 'https://69f8c4e0f7044aa0103e75a0.mockapi.io/api/employees';

export default {
  name: 'EmployeeApp',

  data() {
    return {
      employees: [],
      loading: false,
      isEditing: false,
      editId: null,
      form: {
        name: '',
        designation: '',
        department: '',
        salary: ''
      },
      alert: {
        message: '',
        type: 'success'
      }
    };
  },

  mounted() {
    this.fetchEmployees();
  },

  methods: {

    // ── READ ──────────────────────────────────────
    async fetchEmployees() {
      this.loading = true;
      try {
        const res = await axios.get(API_URL);
        this.employees = res.data;
      } catch (err) {
        this.showAlert('Failed to fetch employees. Check your API URL.', 'danger');
        console.error(err);
      } finally {
        this.loading = false;
      }
    },

    // ── CREATE / UPDATE ───────────────────────────
    async submitForm() {
      if (!this.form.name || !this.form.designation || !this.form.department || !this.form.salary) {
        this.showAlert('Please fill in all fields before submitting.', 'warning');
        return;
      }

      try {
        if (this.isEditing) {
          // PUT - Update existing employee
          await axios.put(`${API_URL}/${this.editId}`, this.form);
          this.showAlert(`Employee "${this.form.name}" updated successfully! ✅`, 'success');
        } else {
          // POST - Create new employee
          await axios.post(API_URL, this.form);
          this.showAlert(`Employee "${this.form.name}" added successfully! 🎉`, 'success');
        }

        this.resetForm();
        this.fetchEmployees();

      } catch (err) {
        this.showAlert('Operation failed. Please try again.', 'danger');
        console.error(err);
      }
    },

    // ── DELETE ────────────────────────────────────
    async deleteEmployee(id) {
      if (!confirm('Are you sure you want to delete this employee?')) return;

      try {
        await axios.delete(`${API_URL}/${id}`);
        // Remove from local list immediately for responsive feel
        this.employees = this.employees.filter(emp => emp.id !== id);
        this.showAlert('Employee deleted successfully.', 'success');
      } catch (err) {
        this.showAlert('Delete failed. Please try again.', 'danger');
        console.error(err);
      }
    },

    // ── EDIT HELPERS ──────────────────────────────
    editEmployee(emp) {
      this.isEditing = true;
      this.editId = emp.id;
      this.form = {
        name: emp.name,
        designation: emp.designation,
        department: emp.department,
        salary: emp.salary
      };
      // Scroll to form
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },

    cancelEdit() {
      this.isEditing = false;
      this.editId = null;
      this.resetForm();
    },

    resetForm() {
      this.isEditing = false;
      this.editId = null;
      this.form = { name: '', designation: '', department: '', salary: '' };
    },

    // ── ALERT HELPER ──────────────────────────────
    showAlert(message, type = 'success') {
      this.alert = { message, type };
      setTimeout(() => { this.alert.message = ''; }, 4000);
    }
  }
};
</script>

<style scoped>
.card {
  border-radius: 12px;
  overflow: hidden;
}

.card-header {
  padding: 16px 20px;
}

.table th {
  font-weight: 600;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.table td {
  vertical-align: middle;
}

.btn {
  border-radius: 8px;
}

.form-control,
.form-select {
  border-radius: 8px;
  border: 1px solid #ced4da;
  transition: box-shadow 0.2s;
}

.form-control:focus,
.form-select:focus {
  box-shadow: 0 0 0 3px rgba(13, 110, 253, 0.15);
}
</style>