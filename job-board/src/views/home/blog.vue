<template>
  <div class="blog-page">
    <!-- Navigation -->
<Navbar></Navbar>
    <!-- Hero Section -->
    <section class="hero-section position-relative text-white py-5">
      <div class="hero-overlay"></div>
      <div class="container text-center py-5">
        <nav aria-label="breadcrumb">
          <!-- Breadcrumb content can be added here if needed -->
        </nav>
        <h1 class="display-4 fw-bold mb-0">Our Blog</h1>
        <p class="lead mt-3">Discover career tips, industry insights, and more.</p>
      </div>
    </section>

    <!-- Blog Section -->
    <section class="blog-section py-5 bg-light">
      <div class="container">
        <div v-if="loading" class="text-center my-5">
          <div class="spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          <p class="mt-2">Loading blogs...</p>
        </div>
        <div v-else-if="error" class="alert alert-danger text-center" role="alert">
          {{ error }}
        </div>
        <div v-else class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
          <div class="col" v-for="blog in paginatedBlogs" :key="blog.id">
            <div class="card h-100 shadow-sm border-0 blog-card">
              <router-link :to="'/blog/' + blog.id" class="card-img-top overflow-hidden">
                <img
                  :src="blog.image"
                  class="img-fluid blog-image"
                  :alt="blog.title"
                  loading="lazy"
                />
              </router-link>
              <div class="card-body d-flex flex-column">
                <div class="text-muted small mb-2 d-flex flex-wrap gap-2">
                  <span><i class="bi bi-calendar3 me-1"></i>{{ blog.date }}</span>
                  <span><i class="bi bi-person me-1"></i>{{ blog.author }}</span>
                  <span><i class="bi bi-chat me-1"></i>{{ blog.comments }}</span>
                </div>
                <h3 class="card-title h5 fw-bold">
                  <router-link
                    :to="'/blog/' + blog.id"
                    class="text-decoration-none text-dark stretched-link"
                  >
                    {{ blog.title }}
                  </router-link>
                </h3>
                <div class="mt-auto d-flex flex-wrap gap-2">
                  <span
                    v-for="(tag, index) in (blog.tags || [blog.category]).slice(0, 3)"
                    :key="tag"
                    class="badge bg-primary"
                  >
                    {{ tag }}
                  </span>
                  <span
                    v-if="(blog.tags || [blog.category]).length > 3"
                    class="badge bg-secondary"
                  >
                    +{{ (blog.tags || [blog.category]).length - 3 }}
                  </span>
                </div>
              </div>
              <div class="card-footer bg-transparent border-0">
                <router-link
                  :to="'/blog/' + blog.id"
                  class="btn btn-outline-primary w-100"
                  aria-label="Read blog post"
                >
                  Read More
                </router-link>
              </div>
            </div>
          </div>
        </div>
        <div class="row mt-5" v-if="paginatedBlogs.length > 0">
          <div class="col text-center">
            <nav aria-label="Page navigation">
              <ul class="pagination justify-content-center gap-2">
                <li class="page-item" :class="{ disabled: currentPage === 1 }">
                  <button
                    class="page-link rounded"
                    @click="changePage(currentPage - 1)"
                    aria-label="Previous page"
                  >
                    <span aria-hidden="true">«</span>
                  </button>
                </li>
                <li
                  class="page-item"
                  v-for="page in totalPages"
                  :key="page"
                  :class="{ active: page === currentPage }"
                >
                  <button
                    class="page-link rounded"
                    @click="changePage(page)"
                    :aria-current="page === currentPage ? 'page' : undefined"
                  >
                    {{ page }}
                  </button>
                </li>
                <li class="page-item" :class="{ disabled: currentPage === totalPages }">
                  <button
                    class="page-link rounded"
                    @click="changePage(currentPage + 1)"
                    aria-label="Next page"
                  >
                    <span aria-hidden="true">»</span>
                  </button>
                </li>
              </ul>
            </nav>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-dark text-white py-5">
      <div class="container">
        <div class="row g-4">
          <div class="col-md-4">
            <h5 class="fw-bold">Skillhunt Jobboard</h5>
            <p class="text-muted">
              Far far away, behind the word mountains, far from the countries Vokalia and Consonantia.
            </p>
            <ul class="list-inline">
              <li class="list-inline-item">
                <a href="#" class="text-white" aria-label="Twitter"><i class="bi bi-twitter"></i></a>
              </li>
              <li class="list-inline-item">
                <a href="#" class="text-white" aria-label="Facebook"><i class="bi bi-facebook"></i></a>
              </li>
              <li class="list-inline-item">
                <a href="#" class="text-white" aria-label="Instagram"><i class="bi bi-instagram"></i></a>
              </li>
            </ul>
          </div>
          <div class="col-md-2">
            <h5 class="fw-bold">Employers</h5>
            <ul class="list-unstyled text-muted">
              <li><a href="#" class="text-decoration-none">Browse Candidates</a></li>
              <li><a href="#" class="text-decoration-none">Post a Job</a></li>
              <li><a href="#" class="text-decoration-none">Employer Listing</a></li>
            </ul>
          </div>
          <div class="col-md-2">
            <h5 class="fw-bold">Candidate</h5>
            <ul class="list-unstyled text-muted">
              <li><a href="#" class="text-decoration-none">Browse Jobs</a></li>
              <li><a href="#" class="text-decoration-none">Submit Resume</a></li>
              <li><a href="#" class="text-decoration-none">Dashboard</a></li>
            </ul>
          </div>
          <div class="col-md-4">
            <h5 class="fw-bold">Have a Question?</h5>
            <ul class="list-unstyled text-muted">
              <li><i class="bi bi-geo-alt me-2"></i>203 Fake St., San Francisco, CA</li>
              <li><i class="bi bi-phone me-2"></i><a href="tel:+23923929210">+2 392 3929 210</a></li>
              <li>
                <i class="bi bi-envelope me-2"></i
                ><a href="mailto:info@yoursite.com">info@yoursite.com</a>
              </li>
            </ul>
          </div>
        </div>
        <div class="row mt-4">
          <div class="col text-center">
            <p class="text-muted">
              Copyright © {{ new Date().getFullYear() }} All rights reserved | Made with
              <i class="bi bi-heart-fill text-danger"></i> by
              <a href="https://colorlib.com" target="_blank" class="text-white">Colorlib</a>
            </p>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script scoped>
import Navbar from '@/components/shared/navbar.vue';

</script>

<style scoped lang="scss">
@use "sass:color";

$primary-color: #007bff;
$accent-color: #ff69b4;
$dark-bg: #1a252f;
$light-bg: #f8f9fa;
$text-color: #212529;
$muted-color: #6c757d;
$shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
$shadow-hover: 0 8px 24px rgba(0, 0, 0, 0.15);

.blog-page {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: $light-bg;
  line-height: 1.6;
}

// Navigation
.navbar {
  transition: background-color 0.3s ease;
  background-color: #fff !important;
  .navbar-brand {
    font-size: 1.5rem;
    font-weight: 700;
    letter-spacing: 0.5px;
    color: $text-color;
    &:hover {
      color: $primary-color;
    }
  }
  .nav-link {
    font-weight: 500;
    padding: 0.5rem 1rem;
    transition: color 0.3s ease;
    color:  #1a252f;;
    &:hover,
    &.active {
      color: $primary-color !important;
    }
  }
  .navbar-toggler {
    border: none;
    &:focus {
      box-shadow: none;
    }
  }
  .dropdown {
    .btn {
      border-radius: 8px;
      font-weight: 500;
      transition: all 0.3s ease;
      &:hover {
        background-color: $primary-color;
        border-color: $primary-color;
        color: #fff;
      }
    }
    .dropdown-menu {
      border-radius: 8px;
      box-shadow: $shadow;
      .dropdown-item {
        font-size: 0.9rem;
        transition: background-color 0.3s ease;
        &:hover {
          background-color: $light-bg;
          color: $primary-color;
        }
      }
      .dropdown-divider {
        border-color: $muted-color;
      }
    }
  }
}

// Hero Section
.hero-section {
  background: linear-gradient(135deg, $primary-color, $accent-color);
  min-height: 300px;
  .hero-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    z-index: 1;
  }
  .container {
    position: relative;
    z-index: 2;
  }
  h1 {
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    animation: fadeInUp 0.6s ease-out;
  }
  .lead {
    opacity: 0.9;
    animation: fadeInUp 0.8s ease-out 0.2s;
    animation-fill-mode: both;
  }
  .breadcrumb {
    a {
      color: #fff;
      text-decoration: none;
      &:hover {
        color: $accent-color;
      }
    }
  }
}

// Blog Section
.blog-section {
  padding: 4rem 0;
  .blog-card {
    border-radius: 12px;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    &:hover {
      transform: translateY(-8px);
      box-shadow: $shadow-hover;
      .blog-image {
        transform: scale(1.05);
      }
    }
    .blog-image {
      height: 200px;
      width: 380px;
      object-fit: cover;
      transition: transform 0.5s ease;
    }
    .card-body {
      padding: 1.5rem;
      .text-muted {
        font-size: 0.85rem;
        i {
          color: $primary-color;
        }
      }
      .card-title {
        margin: 0.5rem 0;
        line-height: 1.4;
        a {
          color: $text-color;
          transition: color 0.3s ease;
          &:hover {
            color: $primary-color;
          }
        }
      }
      .badge {
        font-size: 0.8rem;
        padding: 0.4rem 0.8rem;
        border-radius: 12px;
        transition: background-color 0.3s ease;
        &:hover {
          background-color: color.adjust($primary-color, $lightness: -10%);
        }
      }
    }
    .card-footer {
      padding: 0 1.5rem 1.5rem;
      .btn {
        border-radius: 8px;
        font-weight: 500;
        transition: all 0.3s ease;
        &:hover {
          background-color: $primary-color;
          color: #fff;
          border-color: $primary-color;
          transform: translateY(-2px);
        }
      }
    }
  }
  .pagination {
    .page-link {
      border-radius: 8px;
      margin: 0 0.25rem;
      font-weight: 500;
      transition: all 0.3s ease;
      &:hover {
        background-color: $primary-color;
        color: #fff;
        border-color: $primary-color;
      }
      &.active {
        background-color: $primary-color;
        border-color: $primary-color;
        color: #fff;
      }
    }
    .page-item.disabled .page-link {
      opacity: 0.5;
      cursor: not-allowed;
    }
  }
}

// Footer
footer {
  background: $dark-bg;
  a {
    color: #adb5bd;
    text-decoration: none;
    transition: color 0.3s ease;
    &:hover {
      color: $accent-color;
    }
  }
  .list-inline-item a {
    font-size: 1.2rem;
    transition: transform 0.3s ease;
    &:hover {
      transform: scale(1.2);
    }
  }
}

// Animations
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

// Responsive Adjustments
@media (max-width: 991.98px) {
  .navbar {
    .dropdown {
      margin-right: 1rem;
    }
    .blog-section .row {
      row-gap: 2rem;
    }
  }
}

@media (max-width: 767.98px) {
  .hero-section {
    min-height: 250px;
    h1 {
      font-size: 2.5rem;
    }
    .lead {
      font-size: 1rem;
    }
  }
  .blog-section {
    padding: 2rem 0;
    .blog-card .blog-image {
      height: 180px;
    }
    .pagination .page-link {
      padding: 0.5rem 0.75rem;
      font-size: 0.9rem;
    }
  }
  .navbar {
    .dropdown {
      width: 100%;
      .btn {
        width: 100%;
        justify-content: center;
      }
    }
  }
}

@media (max-width: 575.98px) {
  .navbar-brand {
    font-size: 1.25rem;
  }
}
</style>