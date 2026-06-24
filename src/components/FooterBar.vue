<template>
  <footer class="footer">
    <div class="footer-inner">
      <p class="footer-course">07-034-266 การสร้างธุรกิจเริ่มต้นด้วยนวัตกรรมและเทคโนโลยี</p>
      <p class="footer-instructor">อาจารย์ ดร.สุรเชษฐ์ สังขพันธ์</p>
      <p class="footer-faculty">คณะวิทยาการจัดการ · มหาวิทยาลัยนราธิวาสราชนครินทร์</p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: var(--text);
  color: var(--light);
  text-align: center;
  padding: 2rem 1.5rem;
  margin-top: 2rem;
  font-size: 0.9rem;
  line-height: 1.8;
}

.footer-inner {
  max-width: 800px;
  margin: 0 auto;
}

.footer-course {
  font-weight: 700;
  color: var(--accent);
  margin-bottom: 0.25rem;
}

.footer-instructor {
  color: white;
}

.footer-faculty {
  color: var(--light);
  font-size: 0.8rem;
}
</style>
