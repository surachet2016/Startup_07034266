<script setup>
defineProps({
  week: {
    type: Object,
    required: true,
  },
})

function slideFile(weekNum) {
  return `/slides/Week${String(weekNum).padStart(2, '0')}_Startup.pptx`
}

function slideFileName(weekNum) {
  return slideFile(weekNum).split('/').pop()
}
</script>

<template>
  <section :id="`week-${week.week}`" class="week-section">

    <!-- Header -->
    <div class="week-header">
      <div class="week-badge">สัปดาห์ที่ {{ week.week }}</div>
      <div class="week-titles">
        <h2 class="week-title-th">{{ week.title_th }}</h2>
        <p class="week-title-en">{{ week.title_en }}</p>
      </div>
    </div>

    <!-- CLO / Method / Media chips -->
    <div class="week-chips">
      <div class="chip">
        <span class="chip-label">CLOs</span>
        <span class="chip-value">{{ week.clos }}</span>
      </div>
      <div class="chip">
        <span class="chip-label">วิธีการสอน</span>
        <span class="chip-value">{{ week.method }}</span>
      </div>
      <div class="chip">
        <span class="chip-label">สื่อ</span>
        <span class="chip-value">{{ week.media }}</span>
      </div>
    </div>

    <!-- Slide cards -->
    <div class="slides-grid">
      <div v-for="(slide, i) in week.slides" :key="i" class="slide-card">
        <div class="slide-card-header">{{ slide.title }}</div>
        <ul class="slide-bullets" role="list">
          <li
            v-for="(b, j) in slide.bullets"
            :key="j"
            :class="`bullet-${b.type}`"
          >{{ b.text }}</li>
        </ul>
        <p v-if="slide.note" class="slide-note"><span aria-hidden="true">💡</span> {{ slide.note }}</p>
      </div>
    </div>

    <!-- Activities -->
    <h3 class="section-sub-title">กิจกรรม</h3>
    <div class="activities-grid">
      <div v-for="(act, i) in week.activities" :key="i" class="activity-card">
        <div class="activity-title">{{ act.title }}</div>
        <p class="activity-body">{{ act.body }}</p>
      </div>
    </div>

    <!-- Takeaways -->
    <h3 class="section-sub-title">Key Takeaways</h3>
    <ul class="takeaway-list" role="list">
      <li v-for="(t, i) in week.takeaways" :key="i" class="takeaway-item">
        <span class="checkmark">✓</span> {{ t }}
      </li>
    </ul>

    <!-- Next week preview -->
    <p class="next-week">สัปดาห์หน้า: <strong>{{ week.next_week }}</strong></p>

    <!-- Download button -->
    <a
      :href="slideFile(week.week)"
      :download="slideFileName(week.week)"
      class="download-btn"
    >
      <span aria-hidden="true">⬇</span> ดาวน์โหลดสไลด์ Week {{ week.week }} (.pptx)
    </a>

  </section>
</template>

<style scoped>
.week-section {
  max-width: 1100px;
  margin: 3rem auto;
  padding: 0 1.5rem 3rem;
  border-bottom: 1px solid var(--card-border);
}

.week-header {
  display: flex;
  align-items: flex-start;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.week-badge {
  flex-shrink: 0;
  background: var(--accent);
  color: white;
  font-weight: 700;
  font-size: 0.85rem;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  text-align: center;
  min-width: 110px;
}

.week-title-th {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text);
  line-height: 1.4;
}

.week-title-en {
  font-size: 1rem;
  color: var(--accent);
  margin-top: 0.25rem;
}

.week-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2rem;
}

.chip {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-radius: 8px;
  padding: 0.75rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex: 1;
  min-width: 160px;
  white-space: pre-line;
}

.chip-label {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--highlight);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.chip-value {
  font-size: 0.85rem;
  color: var(--text);
}

.slides-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.slide-card {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-top: 3px solid var(--accent);
  border-radius: 8px;
  padding: 1rem 1.25rem;
}

.slide-card-header {
  font-weight: 700;
  font-size: 1rem;
  color: var(--text);
  margin-bottom: 0.75rem;
}

.slide-bullets {
  list-style: none;
  font-size: 0.875rem;
  line-height: 1.7;
}

.bullet-h1 {
  font-weight: 700;
  color: var(--accent);
  margin-top: 0.5rem;
}

.bullet-h2 {
  font-weight: 700;
  color: var(--highlight);
  margin-top: 0.75rem;
  font-size: 0.95rem;
}

.bullet-bullet::before {
  content: '• ';
  color: var(--accent);
}

.slide-note {
  margin-top: 0.75rem;
  font-size: 0.8rem;
  color: var(--highlight);
  font-style: italic;
  border-top: 1px solid var(--card-border);
  padding-top: 0.5rem;
}

.section-sub-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 1rem;
  padding-left: 0.75rem;
  border-left: 4px solid var(--accent);
}

.activities-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.activity-card {
  background: var(--card);
  border: 1px solid var(--card-border);
  border-top: 3px solid var(--highlight);
  border-radius: 8px;
  padding: 1rem;
}

.activity-title {
  font-weight: 700;
  font-size: 0.9rem;
  color: var(--highlight);
  margin-bottom: 0.5rem;
}

.activity-body {
  font-size: 0.85rem;
  color: var(--text);
  white-space: pre-line;
  line-height: 1.6;
}

.takeaway-list {
  list-style: none;
  margin-bottom: 1rem;
}

.takeaway-item {
  display: flex;
  align-items: baseline;
  gap: 0.5rem;
  font-size: 0.95rem;
  padding: 0.4rem 0;
  border-bottom: 1px dashed var(--card-border);
}

.checkmark {
  color: var(--accent);
  font-weight: 700;
  flex-shrink: 0;
}

.next-week {
  font-size: 0.9rem;
  color: var(--text);
  opacity: 0.6;
  margin: 1rem 0 1.5rem;
}

.download-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: var(--accent);
  color: white;
  font-weight: 700;
  font-size: 0.95rem;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  text-decoration: none;
  transition: background 0.2s, transform 0.1s;
}

.download-btn:hover {
  background: #009bb8;
  transform: translateY(-1px);
}
</style>
