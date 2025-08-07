<style scoped>
  b {
    font-size: 16pt;
    font-weight: bold;
  }

  i {
    margin: 5px;
  }

  .footer {
    max-height: 400px;
    background: rgba(255, 255, 255, 0.3);
    padding: 5px 0;
    font-size: 11pt;
    overflow: hidden;
  }

  .text-muted {
    font-size: 12pt;
    color: #6c757d;
  }

  .footer-bottom {
    text-align: center;
    padding: 10px 0;
    border-top: 1px solid #ddd;
    margin-top: 20px;
    font-size: 12pt;
    color: #6c757d;
  }
</style>

<template>
  <footer class="container-fluid max-w-1280 mt-5 footer">

    <div class="footer-bottom text-center">
      <div class="text-muted">Quản lý Mượn Sách © 2025.</div>
    </div>

  </footer>
</template>

<script>
  export default {
    data() {
      return {
        showDieuKhoan: false,
        showHuongDan: false
      }
    }
  }
</script>
