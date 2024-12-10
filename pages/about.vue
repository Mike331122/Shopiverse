<template>
    <div class="about-container">
        <section class="hero-section">
            <h1 class="hero-title">Welcome to Shopiverse</h1>
            <p class="hero-subtitle">
            Your one-stop shop for exploring and discovering a variety of amazing products!
            </p>
        </section>

        <section class="about-content">
            <h2 class="section-title">What is Shopiverse?</h2>
            <p class="section-description">
            Shopiverse is a demo e-commerce platform created to demonstrate the possibilities
            of modern online shopping. This application simulates a shopping experience where
            users can browse products, explore categories, and visualize the process of adding
            items to a cart.
            </p>
        </section>

        <section class="categories">
            <h2 class="section-title">Explore Our Categories</h2>
            <div class="category-grid">
            <div class="category-card">
                <p class="category-name">Electronics</p>
            </div>
            <div class="category-card">
                <p class="category-name">Jewelery</p>
            </div>
            <div class="category-card">
                <p class="category-name">Men's Clothing</p>
            </div>
            <div class="category-card">
                <p class="category-name">Women's Clothing</p>
            </div>
            </div>
        </section>
    </div>
</template>

<style>
    .about-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  color: #333;
}

.hero-section {
  text-align: center;
  margin-bottom: 40px;
}

.hero-title {
  font-size: 2.5rem;
  font-weight: bold;
  color: #2c3e50;
}

.hero-subtitle {
  font-size: 1.2rem;
  color: #7f8c8d;
  margin-top: 10px;
}

.about-content {
  margin-bottom: 40px;
}

.section-title {
  font-size: 1.8rem;
  color: #2c3e50;
  margin-bottom: 15px;
}

.section-description {
  font-size: 1.1rem;
  color: #7f8c8d;
  line-height: 1.6;
}

.categories {
  text-align: center;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.category-card {
  background-color: #ecf0f1;
  border: 1px solid #bdc3c7;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.category-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.category-name {
  font-size: 1rem;
  font-weight: bold;
  color: #2c3e50;
  text-transform: capitalize;
}
</style>

<script setup>

</script>