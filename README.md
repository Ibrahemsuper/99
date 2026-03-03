let quranData = [];

// تحميل بيانات القرآن
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
  const key = `${surahNum}-${ayahIdx}`;
  if(!memorized.includes(key)) memorized.push(key);
  localStorage.setItem("memorizedAyahs", JSON.stringify(memorized));
  alert("تم حفظ الآية!");
}

// Dark / Light mode
document.getElementById("toggle-theme").addEventListener("click", () => {
  document.body.classList.toggle("dark-mode");
});

// البحث عن كلمة أو اسم السورة
document.getElementById("search-btn").addEventListener("click", () => {
  const query = document.getElementById("search-input").value.trim();
  if (!query) return displaySurahs(quranData);

  const results = [];
  quranData.forEach(surah => {
    // تحقق من وجود الكلمة في نص الآيات أو اسم السورة
    const matchingAyahs = surah.ayahs.filter(ayah =>
      ayah.text.includes(query)
    );
    if (matchingAyahs.length > 0 || surah.name.includes(query)) {
      results.push({ 
        name: surah.name, 
        ayahs: matchingAyahs.length > 0 ? matchingAyahs : [] 
      });
    }
  });

  displaySurahs(results);
});
