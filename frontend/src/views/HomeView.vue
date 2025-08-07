<style scoped>
  h2 {
    color: #5a4631;
  }

  .card {
    max-width: 500px;
    margin: auto;
    border-radius: 15px;
  }

  .sach-list {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    margin-top: 20px;
    justify-content: center;
  }

  .pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }

  .pagination button {
    padding: 8px 12px;
    margin: 5px;
    border: none;
    background: #b89e25;
    color: white;
    cursor: pointer;
    border-radius: 5px;
  }

  .pagination button:disabled {
    background: #ccc;
    cursor: not-allowed;
  }

  .pagination span {
    margin: 15px;
  }

  .carousel-caption {
    position: absolute;
    bottom: 20%;
    left: 50%;
    right: -50%;
    transform: translate(-50%, -50%);
    text-align: center;
    color: white;
    text-shadow: 2px 2px 10px rgba(255, 255, 255, 0.8);
  }

  .carousel-caption h2 {
    color: white;
  }

  .carousel-container {
    display: flex;
    gap: 20px;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }

  .carousel-wrapper {
    width: 100%;
  }

  .overlay-slide {
    height: 420px;
    object-fit: cover;
    filter: brightness(0.7);
    border-radius: 0px;
  }

  .side-image {
    width: 100%;
    height: calc(400px / 3);
    object-fit: cover;
    border-radius: 0px;
  }
</style>

<template>
  <div class="container text-center mt-5">
    <div class="carousel-container">
      <div class="carousel-wrapper">
        <div
          id="carouselExample"
          class="carousel slide carousel-fade"
          data-bs-ride="false"
        >
          <div class="carousel-inner">
            <!-- Slide 1: VIDEO -->
            <div class="carousel-item active">
              <video
                class="d-block w-100 overlay-slide"
                autoplay
                muted
                loop
                playsinline
              >
                <source src="../assets/history_book.mp4" type="video/mp4" />
                Trình duyệt của bạn không hỗ trợ video.
              </video>
              <div class="carousel-caption">
                <h2>Chào mừng đến với thư viện số của bạn</h2>
                <p>Cùng nhau lan tỏa tri thức và cảm hứng đọc sách!</p>
              </div>
            </div>

            <!-- Slide 2 -->
            <div class="carousel-item">
              <img
                src="../assets/slide1.jpg"
                class="d-block w-100 overlay-slide"
                alt="Slide 2"
              />
              <div class="carousel-caption">
                <h2>Khám phá thư viện thông minh</h2>
                <p>Truy cập nhanh chóng, mượn sách tiện lợi chỉ trong vài giây</p>
              </div>
            </div>

            <!-- Slide 3 -->
            <div class="carousel-item">
              <img
                src="../assets/slide2.jpg"
                class="d-block w-100 overlay-slide"
                alt="Slide 3"
              />
              <div class="carousel-caption">
                <h2>Lưu giữ lịch sử đọc của bạn</h2>
                <p>Hệ thống tự động ghi nhận và hỗ trợ quản lý mượn trả hiệu quả</p>
              </div>
            </div>
          </div>

          <!-- Điều hướng trái phải -->
          <button
            class="carousel-control-prev"
            type="button"
            data-bs-target="#carouselExample"
            data-bs-slide="prev"
          >
            <span class="carousel-control-prev-icon"></span>
          </button>
          <button
            class="carousel-control-next"
            type="button"
            data-bs-target="#carouselExample"
            data-bs-slide="next"
          >
            <span class="carousel-control-next-icon"></span>
          </button>
        </div>
      </div>
    </div>

    <InputSearch v-model="search" class="mt-4" />

    <div class="sach-list">
      <SachCard
        v-for="sach in paginatedBooks"
        :key="sach.MASACH"
        :sach="sach"
        :nxbs="nxbs"
      />
    </div>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">
        <i class="bi bi-chevron-left"></i>
        Trước
      </button>
      <span>Trang {{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage >= totalPages">
        Sau
        <i class="bi bi-chevron-right"></i>
      </button>
    </div>
  </div>
</template>

<script>
  import SachCard from '@/components/SachCard.vue'
  import InputSearch from '@/components/InputSearch.vue'
  import { fetchBooks } from '@/services/sachService'
  import { fetchNXB } from '@/services/nxbService'
  import * as bootstrap from 'bootstrap'

  export default {
    components: {
      SachCard,
      InputSearch
    },
    data() {
      return {
        sachList: [],
        search: '',
        nxbs: [],
        currentPage: 1,
        pageSize: 12
      }
    },
    mounted() {
      this.loadBooks()
      this.loadNXBs()
      this.$nextTick(() => {
        const carouselElement = document.querySelector('#carouselExample')
        if (carouselElement) {
          new bootstrap.Carousel(carouselElement, {
            interval: 90000,
            ride: 'carousel'
          })
        }
      })
    },
    computed: {
      userRole() {
        return this.$store.state.user.role
      },
      filteredBooks() {
        return this.sachList.filter(book => {
          const keyword = this.search.toLowerCase().trim()
          const manxbValue = book.MANXB?._id || book.MANXB?.MANXB || ''

          const nxb = this.nxbs.find(n => String(n._id) === String(manxbValue))
          const tenNXB = nxb ? nxb.TENNXB.toLowerCase() : ''
          return (
            book.TENSACH?.toLowerCase().includes(keyword) ||
            book.MASACH?.toLowerCase().includes(keyword) ||
            book.NGUONGOC_TACGIA?.toLowerCase().includes(keyword) ||
            (String(book.NAMXUATBAN) || '').toLowerCase().includes(keyword) ||
            tenNXB.includes(keyword)
          )
        })
      },

      totalPages() {
        return Math.ceil(this.filteredBooks.length / this.pageSize)
      },

      paginatedBooks() {
        const start = (this.currentPage - 1) * this.pageSize
        const end = start + this.pageSize
        return this.filteredBooks.slice(start, end)
      }
    },
    methods: {
      async loadBooks() {
        try {
          this.sachList = await fetchBooks()
          this.sachList = this.shuffleArray(this.sachList)
        } catch (error) {
          console.error('Lỗi khi tải sách:', error)
        }
      },
      shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1))
          ;[array[i], array[j]] = [array[j], array[i]]
        }
        return array
      },
      async loadNXBs() {
        try {
          this.nxbs = await fetchNXB()
        } catch (error) {
          console.error('Lỗi khi tải nhà xuất bản:', error)
        }
      },

      prevPage() {
        if (this.currentPage > 1) {
          this.currentPage--
        }
      },

      nextPage() {
        if (this.currentPage < this.totalPages) {
          this.currentPage++
        }
      }
    }
  }
</script>
