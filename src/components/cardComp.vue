<script setup>
import { ref } from 'vue';
import { onMounted } from 'vue';
import star from '../assets/images/Star.svg'
import star_fill from '../assets/images/Star_fill.svg'
import vector from '../assets/images/vector.svg'
const isActive = ref(false)
const isClicked = ref(false)

const data = ref([])

const ToggleNav = () => {
    isActive.value = !isActive.value
    isClicked.value = false
}

const ToggleNavTwo = () => {
    isActive.value = false;
    isClicked.value = !isClicked.value
}

const fetchCoffee = async () => {
    try {
        const api = "https://raw.githubusercontent.com/devchallenges-io/web-project-ideas/main/front-end-projects/data/simple-coffee-listing-data.json"
        const response = await fetch(api);

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        const result = await response.json();
        data.value = result.map(item => ({ ...item, imgSrc: star }));
        console.log(result)
    } catch (error) {
        console.log(error)
    }
}

const changeSrc = (item) => {
    item.imgSrc = item.imgSrc === star ? star_fill : star
}

onMounted(() => {
    fetchCoffee();
});

</script>
<template>
    <main>
        <div class="header"></div>
        <div class="comp container">
            <div class=" text-center">
                <div class="text-light text-capitalize">
                    <h1>Our collection</h1>
                </div>
                <div class="vector">
                    <img :src="vector" alt="">
                </div>
                <div class="w-50 intro mx-auto text-light-emphasis fw-semibold text-capitalize">
                    <p>introducing our coffee collection, a selection of unique coffees from different roast types and
                        origins.
                        expertly roasted in small batches and shipped fresh weekly</p>
                </div>
            </div>
            <div class="d-flex align-items-center orders justify-content-center pt-lg-3 py-3 py-lg-0">
                <div>
                    <a @click="ToggleNav" class="btn text-light fw-semibold" :class="{ btnsecondary: isActive }"
                        href="#">All products</a>
                </div>
                <div>
                    <a @click="ToggleNavTwo" class="btn text-light fw-semibold" :class="{ btnsecondary: isClicked }"
                        href="#">Available Now</a>
                </div>
            </div>
            <div class="container my-lg-4">
                <div class="row">
                    <div class="text-light d-flex align-items-center justify-content-center col-lg-4 col-md-6"
                        v-for="tea in data" :key="tea.id">
                        <div class="card my-lg-3 my-3 border-0 bg-transparent" style="width: 18rem;">
                            <img :src="tea.image" class="card-img-top rounded-3" alt="...">
                            <div class="card-body py-2 px-1 d-flex align-items-center justify-content-between">
                                <p class="card-text text-light fw-semibold m-0">{{ tea.name }}</p>
                                <p class="card-text text-dark fw-semibold price px-2 rounded-2 m-0">{{ tea.price }}</p>
                            </div>
                            <div class="d-flex align-items-center justify-content-between">
                                <div class="d-flex align-items-center ">
                                    <div class="px-lg-1">
                                        <img @click="changeSrc(tea)" :src="tea.imgSrc" alt="starsign">
                                    </div>
                                    <div class="text-light">
                                        <p class="m-0 px-lg-0 ">{{ tea.rating }}
                                            <span class="text-light-emphasis">({{
                                                tea.votes }} votes)
                                            </span>
                                        </p>
                                    </div>
                                </div>
                                <div>
                                    <p class="text-danger fw-semibold text-capitalize m-0" v-if="tea.votes > 120">sold out</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>
<style scoped>
.header {
    background-image: url('../assets/images/bg-cafe.jpg');
    width: 100%;
    height: 300px;
    background-position: center;
    background-repeat: no-repeat;
}

.price {
    background-color: #bde3cc;
}

.btnsecondary {
    --bs-btn-color: #fff;
    --bs-btn-bg: #6c757d;
    --bs-btn-border-color: #6c757d;
    --bs-btn-hover-color: #fff;
    --bs-btn-hover-bg: #5c636a;
    --bs-btn-hover-border-color: #565e64;
    --bs-btn-focus-shadow-rgb: 130, 138, 145;
    --bs-btn-active-color: #fff;
    --bs-btn-active-bg: #565e64;
    --bs-btn-active-border-color: #51585e;
    --bs-btn-active-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
    --bs-btn-disabled-color: #fff;
    --bs-btn-disabled-bg: #6c757d;
    --bs-btn-disabled-border-color: #6c757d;
}

.comp {
    background-color: #1B1D1F;
}

.text-danger{
    color: #ED735D !important;
}

@media screen and (max-width: 768px) {
    .intro {
        width: 100% !important;
    }
    .comp {
        background-color: #1c1d1f;
        padding-top: 100px;
        position: relative;
        top: -100px;
    }
    .vector{
        position: absolute;
        right: -40px;
        top: 30px;
    }
}

@media screen and (min-width: 768px) {
    .comp {
        background-color: #1c1d1f;
        padding-top: 100px;
        position: relative;
        top: -100px;
    }
    .vector {
        position: absolute;
        right: 250px;
        top: 100px;
        z-index: 1;
    }
}

@media screen and ( max-width: 600px) {
    .vector{
        display: none;
    }
}
</style>
