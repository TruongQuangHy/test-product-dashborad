<script setup lang="ts">
import productsData from '~/data/products.json'

interface Product {
  id: number
  name: string
  price: number
  status: string
  image: string
}

const products = ref<Product[]>(productsData)
const selectedProduct = ref<string | null>(null)

const availableCount = computed(() => {
  return products.value.filter(p => p.status === 'available').length
})

const totalPrice = computed(() => {
  return products.value
    .filter(p => p.status === 'available')
    .reduce((sum, p) => sum + p.price, 0)
})

const selectProduct = (productName: string) => {
  selectedProduct.value = productName
}

const formatPrice = (price: number) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(price)
}

const getStatusText = (status: string) => {
  return status === 'available' ? 'Available' : 'Out of Stock'
}

const getStatusColor = (status: string) => {
  return status === 'available' ? 'green' : 'red'
}
</script>

<template>
  <div class="dashboard-container">
    <div class="dashboard-content">
      <div class="header">
        <h1 class="title">Product Dashboard</h1>
        <p class="subtitle">Browse our premium collection</p>
      </div>

      <div v-if="selectedProduct" class="selected-banner">
        <div class="selected-content">
          <div class="selected-info">
            <svg class="icon" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span class="selected-text">Selected: {{ selectedProduct }}</span>
          </div>
          <button @click="selectedProduct = null" class="close-btn">
            <svg class="icon-sm" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
          </button>
        </div>
      </div>

      <div class="products-grid">
        <div
          v-for="product in products"
          :key="product.id"
          class="product-card"
          @click="selectProduct(product.name)"
        >
          <img 
            :src="product.image" 
            :alt="product.name"
            class="product-image"
          />
          <div class="product-info">
            <div class="product-header">
              <h3 class="product-name">{{ product.name }}</h3>
              <span :class="['badge', product.status === 'available' ? 'badge-green' : 'badge-red']">
                {{ getStatusText(product.status) }}
              </span>
            </div>
            <p class="product-price">{{ formatPrice(product.price) }}</p>
            <button class="view-btn">
              <svg class="icon-sm" fill="currentColor" viewBox="0 0 20 20">
                <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
                <path fill-rule="evenodd" d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z" clip-rule="evenodd" />
              </svg>
              View Details
            </button>
          </div>
        </div>
      </div>

      <div class="summary-section">
        <div class="summary-card">
          <div class="summary-header">
            <svg class="icon text-green" fill="currentColor" viewBox="0 0 20 20">
              <path d="M2 11a1 1 0 011-1h2a1 1 0 011 1v5a1 1 0 01-1 1H3a1 1 0 01-1-1v-5zM8 7a1 1 0 011-1h2a1 1 0 011 1v9a1 1 0 01-1 1H9a1 1 0 01-1-1V7zM14 4a1 1 0 011-1h2a1 1 0 011 1v12a1 1 0 01-1 1h-2a1 1 0 01-1-1V4z" />
            </svg>
            <h2 class="summary-title">Sales Summary</h2>
          </div>
          <div class="summary-grid">
            <div class="stat-card">
              <div class="stat-content">
                <div>
                  <p class="stat-label">Available Products</p>
                  <p class="stat-value text-green">{{ availableCount }}</p>
                </div>
                <div class="stat-icon bg-green">
                  <svg class="icon-lg" fill="currentColor" viewBox="0 0 20 20">
                    <path fill-rule="evenodd" d="M10 2a4 4 0 00-4 4v1H5a1 1 0 00-.994.89l-1 9A1 1 0 004 18h12a1 1 0 00.994-1.11l-1-9A1 1 0 0015 7h-1V6a4 4 0 00-4-4zm2 5V6a2 2 0 10-4 0v1h4zm-6 3a1 1 0 112 0 1 1 0 01-2 0zm7-1a1 1 0 100 2 1 1 0 000-2z" clip-rule="evenodd" />
                  </svg>
                </div>
              </div>
            </div>
            <div class="stat-card">
              <div class="stat-content">
                <div>
                  <p class="stat-label">Total Value</p>
                  <p class="stat-value text-blue">{{ formatPrice(totalPrice) }}</p>
                </div>
                <div class="stat-icon bg-blue">
                  <svg class="icon-lg" fill="currentColor" viewBox="0 0 20 20">
                    <path d="M8.433 7.418c.155-.103.346-.196.567-.267v1.698a2.305 2.305 0 01-.567-.267C8.07 8.34 8 8.114 8 8c0-.114.07-.34.433-.582zM11 12.849v-1.698c.22.071.412.164.567.267.364.243.433.468.433.582 0 .114-.07.34-.433.582a2.305 2.305 0 01-.567.267z" />
                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-13a1 1 0 10-2 0v.092a4.535 4.535 0 00-1.676.662C6.602 6.234 6 7.009 6 8c0 .99.602 1.765 1.324 2.246.48.32 1.054.545 1.676.662v1.941c-.391-.127-.68-.317-.843-.504a1 1 0 10-1.51 1.31c.562.649 1.413 1.076 2.353 1.253V15a1 1 0 102 0v-.092a4.535 4.535 0 001.676-.662C13.398 13.766 14 12.991 14 12c0-.99-.602-1.765-1.324-2.246A4.535 4.535 0 0011 9.092V7.151c.391.127.68.317.843.504a1 1 0 101.511-1.31c-.563-.649-1.413-1.076-2.354-1.253V5z" clip-rule="evenodd" />
                  </svg>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dashboard-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #dbeafe 0%, #ffffff 50%, #fae8ff 100%);
  padding: 3rem 1rem;
}

.dashboard-content {
  max-width: 1280px;
  margin: 0 auto;
}

.header {
  text-align: center;
  margin-bottom: 3rem;
}

.title {
  font-size: 2.25rem;
  font-weight: 700;
  color: #111827;
  margin-bottom: 0.5rem;
}

.subtitle {
  font-size: 1.125rem;
  color: #4b5563;
}

.selected-banner {
  margin-bottom: 2rem;
}

.selected-content {
  background: linear-gradient(to right, #3b82f6, #9333ea);
  color: white;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.selected-info {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.selected-text {
  font-size: 1.125rem;
  font-weight: 600;
}

.close-btn {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 0.25rem;
  padding: 0.5rem;
  cursor: pointer;
  transition: background 0.2s;
  color: white;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.products-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
  margin-bottom: 3rem;
}

@media (min-width: 768px) {
  .products-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .products-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

.product-card {
  background: white;
  border-radius: 0.5rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: all 0.3s;
  overflow: hidden;
}

.product-card:hover {
  transform: scale(1.05);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
}

.product-image {
  width: 100%;
  height: 12rem;
  object-fit: cover;
}

.product-info {
  padding: 1.5rem;
}

.product-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 0.75rem;
}

.product-name {
  font-size: 1.25rem;
  font-weight: 600;
  color: #111827;
}

.badge {
  padding: 0.25rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 500;
  border-radius: 9999px;
}

.badge-green {
  background: #d1fae5;
  color: #065f46;
}

.badge-red {
  background: #fee2e2;
  color: #991b1b;
}

.product-price {
  font-size: 1.5rem;
  font-weight: 700;
  color: #2563eb;
  margin-bottom: 0.75rem;
}

.view-btn {
  width: 100%;
  background: #dbeafe;
  color: #1d4ed8;
  border: none;
  border-radius: 0.5rem;
  padding: 0.75rem 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.view-btn:hover {
  background: #bfdbfe;
}

.summary-section {
  margin-top: 3rem;
}

.summary-card {
  background: linear-gradient(to right, #ecfdf5, #dbeafe);
  border-radius: 0.5rem;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.summary-header {
  background: white;
  padding: 1.5rem;
  border-bottom: 1px solid #e5e7eb;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.summary-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #111827;
}

.summary-grid {
  padding: 1.5rem;
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
}

@media (min-width: 768px) {
  .summary-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.stat-card {
  background: white;
  border-radius: 0.5rem;
  padding: 1.5rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.stat-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.stat-label {
  font-size: 0.875rem;
  font-weight: 500;
  color: #4b5563;
  margin-bottom: 0.25rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
}

.stat-icon {
  border-radius: 9999px;
  padding: 1rem;
}

.icon {
  width: 1.5rem;
  height: 1.5rem;
}

.icon-sm {
  width: 1.25rem;
  height: 1.25rem;
}

.icon-lg {
  width: 2rem;
  height: 2rem;
}

.text-green {
  color: #059669;
}

.text-blue {
  color: #2563eb;
}

.bg-green {
  background: #d1fae5;
  color: #059669;
}

.bg-blue {
  background: #dbeafe;
  color: #2563eb;
}
</style>
