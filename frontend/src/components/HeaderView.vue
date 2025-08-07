<style scoped>
body {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
}

.header-container {
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
  backdrop-filter: blur(10px);
  padding: 10px 0;
}

/* New: Container chứa logo + navbar hai bên */
.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
}

/* Logo + tiêu đề */
.branding {
  display: flex;
  align-items: center;
  gap: 12px;
}

.logo {
  width: 60px;
  height: 60px;
  object-fit: contain;
}

.welcome-text {
  font-size: 20pt;
  font-weight: bold;
  color: #4CAF50; /* theo Option 2 */
  margin: 0;
}

.navbar {
  display: flex;
  justify-content: flex-end;
  padding: 8px 0;
  border-radius: 0 0 12px 12px;
  transition: all 0.3s ease;
}

.navbar-nav {
  display: flex;
  align-items: center;
}

.navbar-nav .nav-link {
  font-size: 16px;
  font-weight: 600;
  color: #212121 !important;
  margin: 0 12px;
  transition: all 0.3s ease;
  padding: 12px 18px;
  border-radius: 8px;
}

.navbar-nav .nav-link:hover {
  background: #FFC107;
  color: white !important;
  transform: scale(1.08);
}

.navbar-nav .nav-link[to='/logout'] {
  background: #4CAF50;
  color: #fff !important;
  border-radius: 8px;
  padding: 12px 18px;
}

.navbar-nav .nav-link[to='/logout']:hover {
  background: #388E3C;
  transform: scale(1.08);
}

.navbar-toggler {
  border: none;
  outline: none;
}

.navbar-toggler:focus {
  box-shadow: none;
}

@media (max-width: 992px) {
  .header-content {
    flex-direction: column;
    align-items: flex-start;
    padding: 0 16px;
  }

  .navbar-nav {
    flex-direction: column;
    align-items: flex-start;
    padding-top: 10px;
    width: 100%;
  }

  .navbar-nav .nav-link {
    display: block;
    margin-bottom: 12px;
    width: 100%;
    padding: 12px;
  }
}
</style>


<template>
  <div class="header-container">
    <div class="header-content">
      <!-- Logo và tiêu đề căn trái -->
      <div class="branding">
        <img src="../assets/LOGO_500x500px_transparent.png" alt="Logo" class="logo" />
        <p class="welcome-text">
          <strong>Quản lý Mượn Sách</strong>
        </p>
      </div>

      <!-- Thanh điều hướng căn phải -->
      <nav class="navbar navbar-expand-lg navbar-dark">
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <router-link class="nav-link" to="/">Trang chủ</router-link>
            </li>

            <template v-if="userRole === 'docgia'">
              <li class="nav-item">
                <router-link class="nav-link" to="/muon-sach">Đăng Ký Mượn Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/lich-su-muon">Lịch Sử Mượn</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/tai-khoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else-if="userRole === 'quanly'">
              <li class="nav-item">
                <router-link class="nav-link" to="/quan-ly-sach">Quản Lý Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/quan-ly-tai-khoan">Quản Lý Tài Khoản</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/theo-doi-muon">Quản Lý Mượn Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/tai-khoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else-if="userRole === 'nhanvien'">
              <li class="nav-item">
                <router-link class="nav-link" to="/theo-doi-muon">Quản Lý Mượn Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/tai-khoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else>
              <li class="nav-item">
                <router-link class="nav-link" to="/login-docgia">Đăng nhập</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/register">Đăng ký</router-link>
              </li>
            </template>

            <li class="nav-item" v-if="userRole">
              <router-link class="nav-link" to="/logout" @click="handleLogout">Đăng Xuất</router-link>
            </li>
          </ul>
        </div>
      </nav>
    </div>
  </div>
</template>

<script>
  export default {
    computed: {
      userRole() {
        return this.$store.state.user.role
      }
    },
    methods: {
      handleLogout() {
        this.$store.dispatch('logout')
        this.$router.push('/login-docgia')
      }
    }
  }
</script>