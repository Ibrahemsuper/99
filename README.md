// تحميل بيانات القرآن
let quranData = [];
fetch("quran.json")
  .then(res => res.json())
  .then(data => {
    quranData = data.surahs;
    displaySurahs(quranData);
  });

// عرض السور والآيات
const surahList = document.getElementById("surah-list");
function displaySurahs(surahs) {
  surahList.innerHTML = "";
  surahs.forEach(surah => {
    const surahDiv = document.createElement("div");
    surahDiv.innerHTML = `<h2>${surah.name}</h2>`;
    surah.ayahs.forEach((ayah, idx) => {
      const ayahDiv = document.createElement("div");
      ayahDiv.classList.add("ayah-card");
      ayahDiv.innerHTML = `
        <p>${ayah.text}</p>
        <button onclick="toggleTafsir(${surah.number}, ${idx})">تفسير</button>
        <p id="tafsir-${surah.number}-${idx}" style="display:none;">${ayah.tafsir}</p>
        <button onclick="memorizeAyah(${surah.number}, ${idx})">تم الحفظ</button>
      `;
      surahDiv.appendChild(ayahDiv);
    });
    surahList.appendChild(surahDiv);
  });
}

// إظهار / إخفاء التفسير
function toggleTafsir(surahNum, ayahIdx) {
  const tafsirEl = document.getElementById(`tafsir-${surahNum}-${ayahIdx}`);
  tafsirEl.style.display = tafsirEl.style.display === "none" ? "block" : "none";
}

// تتبع الحفظ
function memorizeAyah(surahNum, ayahIdx) {
  let memorized = JSON.parse(localStorage.getItem("memorizedAyahs") || "[]");
  memorized.push(`${surahNum}-${ayahIdx}`);
  localStorage.setItem("memorizedAyahs", JSON.stringify(memorized));
  alert("تم حفظ الآية!");
}

// Dark / Light mode
document.getElementById("toggle-theme").addEventListener("click", () => {
  document.body.classList.toggle("dark-mode");
});

// بحث
const searchInput = document.getElementById("search-input");
searchInput.addEventListener("input", () => {
  const query = searchInput.value.trim();
  if (!query) return displaySurahs(quranData);

  const results = [];
  quranData.forEach(surah => {
    const matchingAyahs = surah.ayahs.filter(ayah => ayah.text.includes(query));
    if (matchingAyahs.length > 0) {
      results.push({ name: surah.name, ayahs: matchingAyahs });
    }
  });

  displaySurahs(results);
});
