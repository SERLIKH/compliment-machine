<template>
    <div class="h-screen overflow-hidden relative bg-cover bg-center flex items-center justify-center"
        style="background-image: url('/background.png')">
        <!-- Characters -->
        <!-- Characters (Desktop Only) -->
        <div class="hidden xl:block">
            <FloatingCharacter ori="/characters/char1/ori.png" hover="/characters/char1/hover.png"
                :style="{ top: '-8%', left: '-4.5%', width: '430px' }" />

            <FloatingCharacter ori="/characters/char2/ori.png" hover="/characters/char2/hover.png"
                :style="{ top: '-5%', right: '-3%', width: '400px' }" />

            <FloatingCharacter ori="/characters/char3/ori.png" hover="/characters/char3/hover.png"
                :style="{ bottom: '40%', left: '0%', width: '360px' }" />

            <FloatingCharacter ori="/characters/char4/ori.png" hover="/characters/char4/hover.png"
                :style="{ bottom: '45%', right: '-4%', width: '400px' }" />

            <FloatingCharacter ori="/characters/char5/ori.png" hover="/characters/char5/hover.png"
                :style="{ bottom: '25%', left: '50%', transform: 'translateX(-50%)', width: '360px' }" />
        </div>

        <button @click="toggleSound"
            class="absolute top-6 right-6 bg-white/80 backdrop-blur px-4 py-2 rounded-full font-bold shadow hover:scale-105 transition">
            {{ isSoundOn ? '🔊 Sound ON' : '🔇 Sound OFF' }}
        </button>


        <!-- Main Content -->
        <div class="text-center space-y-7 max-w-2xl px-6 z-10">
            <h1 class="text-4xl md:text-5xl lg:text-7xl font-black text-blue-800">
                The Compliment Machine
            </h1>


            <input v-model="name" placeholder="Masukkan Nama-mu..."
                class="w-full border-4 border-dashed border-gray-400 rounded-3xl px-4 py-3 text-center" />

            <p class="text-lg font-medium min-h-[60px]">
                {{ compliment }}
            </p>

            <button @click="generate"
                class="bg-orange-400 hover:bg-yellow-500 text-white font-extrabold px-10 md:px-16 lg:px-20 py-3 md:py-4 rounded-full transition active:scale-95">
                Puji Aku
            </button>

        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import FloatingCharacter from '~/components/FloatingCharacter.vue'

const name = ref('')
const compliment = ref('')
const isSoundOn = ref(false)

let bgAudio
let clickAudio
let confetti

onMounted(async () => {
    // 🎶 Background music
    bgAudio = new Audio('/sounds/bg-music.mp3')
    bgAudio.loop = true
    bgAudio.volume = 0.4

    // 🔘 Click sound
    clickAudio = new Audio('/sounds/click.mp3')
    clickAudio.volume = 0.8

    // 🎉 Confetti
    confetti = (await import('canvas-confetti')).default
})

function toggleSound() {
    isSoundOn.value = !isSoundOn.value

    if (isSoundOn.value) {
        bgAudio.currentTime = 0
        bgAudio.play().catch(() => { })
    } else {
        bgAudio.pause()
    }
}


const compliments = [
    "Asli, setiap ada {name}, suasana langsung jadi seru. Kayak ada energi tambahan gitu!",
    "{name}, kamu itu kalau ngerjain apa-apa kayaknya gampang banget, padahal kita yang liat udah pusing.",
    "Sumpah ya, kalau semua orang kerjanya se-sat-set {name}, dunia ini kelar masalahnya dari kemarin!",
    "{name}, kok kamu bisa sih tetap tenang padahal lagi chaos? Mental kamu juara banget.",
    "Beneran deh, ngobrol sama {name} itu selalu bikin dapet insight baru yang nggak kepikiran.",
    "Heran deh, {name} kalau pakai baju apa aja kenapa bisa selalu pas dan keren gitu ya?",
    "{name}, makasih ya udah jadi orang yang selalu bisa diandelin. Kamu penyelamat banget!",
    "Gak tahu kenapa, tapi kalau ada {name} di tim, aku ngerasa semuanya pasti bakal aman.",
    "Kamu itu tipe orang yang kalau nggak ada, pasti dicariin sama semua orang, {name}.",
    "Gila, ini mah bukan cuma rapi, tapi karya seni! {name} emang nggak main-main kalau kerja.",
    "Tolong kasih tau rahasianya, {name}. Kok bisa teliti banget sampai ke hal-hal kecil?",
    "{name}, jujur ya, presentasi kamu tadi itu standar paling tinggi yang pernah aku liat.",
    "Cara kamu jelasin hal ribet jadi gampang itu bakat langka, {name}.",
    "Dunia kerja butuh lebih banyak orang kayak {name}: cerdas, tapi tetap rendah hati.",
    "Aku belajar banyak banget dari cara kamu ngadepin masalah, {name}. Respect!",
    "{name}, ide kamu tadi bener-bener game changer. Jenius banget!",
    "Jangan-jangan kamu punya mesin waktu ya? Kok bisa nyelesaiin tugas secepat ini, {name}?",
    "Setiap kali {name} ngasih masukan, pasti langsung ngena ke intinya. Tajem banget!",
    "Skill {name} itu udah level yang bikin orang lain bilang, 'Kok bisa kepikiran ya?'",
    "Salut sama {name}, bisa banget bikin orang ngerasa didengerin dan dihargai.",
    "Selera musik/film kamu emang nggak pernah gagal, {name}. Selalu berkelas!",
    "{name}, kamu tuh punya aura yang bikin orang otomatis nyaman kalau di deket kamu.",
    "Kadang suka bingung, {name} ini energinya nggak habis-habis ya buat bikin orang ketawa?",
    "Kamu itu definisi 'keren tanpa usaha', {name}. Semuanya kelihatan alami banget.",
    "Punya temen kayak {name} itu kayak punya support system paling elit.",
    "{name}, cara kamu bawa diri itu bener-bener bikin kagum. Elegan tapi tetep asik.",
    "Kamu itu orang pertama yang aku hubungin kalau butuh saran jujur, makasih ya {name}.",
    "Nggak cuma pinter, tapi {name} itu peduli banget sama orang. Paket lengkap!",
    "Asli, {name}, selera humor kamu itu penyelamat di hari-hari yang ngebosenin.",
    "{name}, kamu sadar nggak sih kalau keberadaan kamu itu motivasi buat orang lain?",
    "Jangan berhenti jadi keren ya, {name}. Dunia butuh lebih banyak orang kayak kamu.",
    "Gila, progres kamu bulan ini pesat banget! Aku bangga banget sama kamu, {name}.",
    "{name}, kamu itu kalau udah fokus, auranya bener-bener nggak bisa diganggu gugat!",
    "Setiap kali kamu mulai ngomong, orang-orang tuh kayak otomatis langsung dengerin. Hebat!",
    "Kamu itu bukan cuma sekadar bisa, tapi kamu menguasai bidang ini, {name}.",
    "Serius deh, nama {name} itu udah jadi jaminan kalau hasilnya bakal luar biasa.",
    "Keren banget cara kamu bangkit lagi pas lagi susah. Kamu kuat banget, {name}.",
    "{name}, makasih ya udah selalu kasih energi positif buat kita semua.",
    "Kalau ada orang yang layak dapet semua keberuntungan di dunia, itu pasti kamu, {name}.",
    "{name}, kamu itu legend di mata aku!",
    "Asli, kamu keren banget hari ini!",
    "Nggak paham lagi, kok bisa ya kamu sehebat itu, {name}?",
    "Kamu itu aset paling berharga yang kita punya, {name}.",
    "Standard aku jadi tinggi gara-gara liat kerjaan kamu, {name}.",
    "Masa depan cerah banget kalau di tangan orang kayak kamu.",
    "{name}, kamu itu juara tanpa perlu piala.",
    "Cuma mau bilang, kamu itu luar biasa. Titik.",
    "Makasih udah jadi versi terbaik diri kamu hari ini, {name}.",
    "Dunia harus tahu kalau {name} itu kerennya nggak masuk akal!"
]


function generate() {
    if (isSoundOn.value && clickAudio) {
        clickAudio.currentTime = 0
        clickAudio.play().catch(() => { })
    }

    const random = Math.floor(Math.random() * compliments.length)
    const userName = name.value || 'Kamu'
    compliment.value = compliments[random].replace('{name}', userName)

    confetti({
        particleCount: 250,
        spread: 120,
        origin: { y: 0.6 }
    })
}
</script>