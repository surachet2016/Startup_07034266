<template>
  <section class="hero">
    <div class="hero-inner">
      <div class="hero-badge">07-034-266</div>
      <h1 class="hero-title-th">การสร้างธุรกิจเริ่มต้นด้วยนวัตกรรมและเทคโนโลยี</h1>
      <h2 class="hero-title-en">Starting a Business with Innovation and Technology</h2>
      <div class="hero-divider" />
      <div class="hero-meta">
        <div class="meta-item">
          <span class="meta-label">ผู้สอน</span>
          <span class="meta-value">อาจารย์ ดร.สุรเชษฐ์ สังขพันธ์</span>
        </div>
        <div class="meta-item">
          <span class="meta-label">สังกัด</span>
          <span class="meta-value">คณะวิทยาการจัดการ มหาวิทยาลัยนราธิวาสราชนครินทร์</span>
        </div>
        <div class="meta-item">
          <span class="meta-label">หน่วยกิต</span>
          <span class="meta-value">3 หน่วยกิต (2-2-5) · 8 สัปดาห์</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  background: linear-gradient(135deg, var(--text) 0%, #1a3a5c 100%);
  color: white;
  padding: 4rem 2rem;
  text-align: center;
}

.hero-inner {
  max-width: 800px;
  margin: 0 auto;
}

.hero-badge {
  display: inline-block;
  background: var(--accent);
  color: var(--text);
  font-weight: 700;
  font-size: 0.9rem;
  padding: 0.3rem 1rem;
  border-radius: 20px;
  margin-bottom: 1.5rem;
}

.hero-title-th {
  font-size: clamp(1.4rem, 3vw, 2.2rem);
  font-weight: 700;
  line-height: 1.4;
  margin-bottom: 0.75rem;
}

.hero-title-en {
  font-size: clamp(0.9rem, 2vw, 1.2rem);
  font-weight: 400;
  color: var(--accent);
  margin-bottom: 2rem;
}

.hero-divider {
  width: 60px;
  height: 3px;
  background: var(--highlight);
  margin: 0 auto 2rem;
  border-radius: 2px;
}

.hero-meta {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
}

.meta-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.meta-label {
  font-size: 0.75rem;
  color: var(--light);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.meta-value {
  font-size: 0.95rem;
  font-weight: 600;
}
</style>
