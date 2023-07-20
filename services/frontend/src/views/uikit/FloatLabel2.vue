<script setup>
import { ref, onMounted } from 'vue';
import CountryService from '@/service/CountryService';
// import ScanerService from '@/service/ScanerService';
import ProductService from '@/service/ProductService';
// import PhotoService from '@/service/PhotoService';
import { useLayout } from '@/layout/composables/layout';
import Image from 'primevue/image';
// import PassportBoxService from '../../service/PassportBoxService';
// import ProgressSpinner from 'primevue/progressspinner';

const { contextPath } = useLayout();

const products = ref([]);
const images = ref([]);
const galleriaResponsiveOptions = ref([
    {
        breakpoint: '1024px',
        numVisible: 5
    },
    {
        breakpoint: '960px',
        numVisible: 4
    },
    {
        breakpoint: '768px',
        numVisible: 3
    },
    {
        breakpoint: '560px',
        numVisible: 1
    }
]);
// const carouselResponsiveOptions = ref([
//     {
//         breakpoint: '1024px',
//         numVisible: 3,
//         numScroll: 3
//     },
//     {
//         breakpoint: '768px',
//         numVisible: 2,
//         numScroll: 2
//     },
//     {
//         breakpoint: '560px',
//         numVisible: 1,
//         numScroll: 1
//     }
// ]);

const productService = new ProductService();
// const photoService = new PhotoService();

const countries = ref([]);
// const scans = ref([]);

// const cities = ref([
//     { name: 'New York', code: 'NY' },
//     { name: 'Rome', code: 'RM' },
//     { name: 'London', code: 'LDN' },
//     { name: 'Istanbul', code: 'IST' },
//     { name: 'Paris', code: 'PRS' }
// ]);
const sex = ref([
    { name: 'Мужской', code: 'M' },
    { name: 'Женский', code: 'F' }
]);
const dul = ref([
    { name: 'Российский паспорт', code: '1' },
    { name: 'Иностранный паспорт', code: '2' },
    { name: 'Заграничный паспорт', code: '3' }
]);
// const filteredCountries = ref(null);
const birthPlace = ref(null);
// const citizenship = ref(null);
const birthDate = ref(null);
// const value4 = ref(null);
// const value5 = ref(null);
const serNumDul = ref(null);
// const value7 = ref(null);
const gender = ref(null); // Пол
// const value9 = ref(null);
const textarea = ref(null);
const lastname = ref(null); // Фамилия
const firstname = ref(null); // Имя
const middlename = ref(null); // Отчество
const valueDul = ref(null);
const dateIssue = ref(null);
const issuedBy = ref(null);
const department = ref(null);
const countryService = new CountryService();
// const scanerService = new ScanerService();
// const passportBoxService = new PassportBoxService();
const selectedCountry = ref(null);

onMounted(() => {
    countryService.getCountries().then((data) => (countries.value = data));
    productService.getProductsSmall().then((data) => (products.value = data));
    // photoService.getImages().then((data) => (images.value = data));
});

// const searchCountry = (event) => {
//     //in a real application, make a request to a remote url with the query and return filtered results, for demo we filter at client side
//     const filtered = [];
//     const query = event.query;
//     for (let i = 0; i < countries.value.length; i++) {
//         const country = countries.value[i];
//         if (country.name.toLowerCase().indexOf(query.toLowerCase()) == 0) {
//             filtered.push(country);
//         }
//     }
//     filteredCountries.value = filtered;
// };

// const state = ref({});

// fetch('http://localhost:8080/TakeSnapshot')
//     .then(response => response.blob())
//     .then(data => state.value = data);

// images.value = state.value;

// function fetchImage() {
//     fetch('http://localhost:8080/TakeSnapshot')
//         .then((response) => response.blob())
//         .then((blob) => {
//             // const imageUrl = URL.createObjectURL(blob);
//             // const imageElement = document.createElement('img');
//             // imageElement.src = imageUrl;
//             // const container = document.getElementById('image-container');
//             // container.appendChild(imageElement);
//             images.value = 'https://cdn.sstatic.net/Img/teams/teams-illo-free-sidebar-promo.svg?v=47faa659a05e';
//         })
//         .catch((error) => console.error(error));
// }

var myimage = ref(null);
myimage.value = 'https://ooopht.ru/assets/uploads/other/no-img.png';

function imageContainer() {
    myimage.value = 'http://localhost:8080/TakeSnapshot';
    console.log(myimage.value);
}

function imageContainerReset() {
    myimage.value = 'https://ooopht.ru/assets/uploads/other/no-img.png';
    console.log(myimage.value);
}

// function getCitizenshipByDocType(doc_type) {
//     if (doc_type == 'rus.passport.national') {
//         return '1';
//     }
// }

function imageOCR() {
    fetch('http://localhost:8080/CaptureDocument')
        .then((response) => response.json())
        .then((data) => {
            // const imageUrl = URL.createObjectURL(blob);
            // const imageElement = document.createElement('img');
            // imageElement.src = imageUrl;
            // const container = document.getElementById('image-container');
            // container.appendChild(imageElement);
            console.log(data);
            lastname.value = data.fields.surname.value;
            firstname.value = data.fields.name.value;
            middlename.value = data.fields.patronymic.value;
            // gender.value = data.fields.gender.value;
            birthDate.value = data.fields.birthdate.value;
            // citizenship.value = getCitizenshipByDocType(data.doc_type.value);
            birthPlace.value = data.fields.birthplace.value;
            // valueDul
            serNumDul.value = data.fields.series.value + ' ' + data.fields.number.value;
            department.value = data.fields.authority_code.value;
            dateIssue.value = data.fields.issue_date.value;
            issuedBy.value = data.fields.authority.value;
            textarea.value = data.doc_type.value;

            // () => {
            //     if (data.fields.gender.value == 'МУЖ.') {
            //         return 'Мужской';
            //     }
            // };
        })
        .catch((error) => console.error(error));
}
</script>

<script>
import pdf from 'vue-pdf'

export default {
  components: {
    pdf
  }
}
</script>

<template>
    <!-- <div class="card">
        <h5>Float Label</h5>
        <div class="grid p-fluid mt-3">
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <InputText type="text" id="inputtext" v-model="birthPlace" />
                    <label for="inputtext">InputText</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <AutoComplete id="autocomplete" v-model="citizenship" :suggestions="filteredCountries" @complete="searchCountry($event)" field="name"></AutoComplete>
                    <label for="autocomplete">AutoComplete</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <Calendar inputId="calendar" v-model="birthDate"></Calendar>
                    <label for="calendar">Calendar</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <Chips inputId="chips" v-model="value4"></Chips>
                    <label for="chips">Chips</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <InputMask id="inputmask" mask="99/99/9999" v-model="value5"></InputMask>
                    <label for="inputmask">InputMask</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <InputNumber id="inputnumber" v-model="serNumDul"></InputNumber>
                    <label for="inputnumber">InputNumber</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <div class="p-inputgroup">
                    <span class="p-inputgroup-addon">
                        <i class="pi pi-user"></i>
                    </span>
                    <span class="p-float-label">
                        <InputText type="text" id="inputgroup" v-model="value7" />
                        <label for="inputgroup">InputGroup</label>
                    </span>
                </div>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <Dropdown id="dropdown" :options="cities" v-model="gender" optionLabel="name"></Dropdown>
                    <label for="dropdown">Dropdown</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <MultiSelect id="multiselect" :options="cities" v-model="value9" optionLabel="name" :filter="false"></MultiSelect>
                    <label for="multiselect">MultiSelect</label>
                </span>
            </div>
            <div class="field col-12 md:col-4">
                <span class="p-float-label">
                    <Textarea inputId="textarea" rows="3" cols="30" v-model="textarea"></Textarea>
                    <label for="textarea">Textarea</label>
                </span>
            </div>
        </div>
    </div> -->
    <div class="progress-spinner" v-if="false">
        <p-progressSpinner></p-progressSpinner>
    </div>
    <div class="grid">
        <div class="col-12 lg:col-12 xl:col-6">
            <div class="card">
                <h5>Документы</h5>
                <!-- <ProgressSpinner style="width: 50px; height: 50px" strokeWidth="8" fill="var(--surface-ground)" animationDuration=".5s" aria-label="Custom ProgressSpinner" /> -->
                <Button label="Сканировать" class="mr-2 mb-2" @click="imageContainer()"></Button>
                <Button label="Очистить" class="mr-2 mb-2" @click="imageContainerReset()"></Button>
                <Button label="Распознать" class="mr-2 mb-2" @click="imageOCR()"></Button>
                <Galleria :value="images" :responsiveOptions="galleriaResponsiveOptions" :numVisible="7" :circular="true" containerStyle="max-width: 800px; margin: auto">
                    <template #item="slotProps">
                        <Image :src="contextPath + slotProps.item.itemImageSrc" :alt="slotProps.item.alt" style="width: 100%; display: flex; justify-content: center; align-items: center; height: 100%; width: 100%" preview />
                    </template>
                    <template #thumbnail="slotProps">
                        <img :src="contextPath + slotProps.item.thumbnailImageSrc" :alt="slotProps.item.alt" tyle="width: 100%; display: block;" />
                    </template>
                </Galleria>
                <!-- <div id="image-container"></div> -->
            </div>
            <div class="card">
                <h5>Скан</h5>
                <div class="flex justify-content-center">
                    <Image :src="myimage" alt="Image" width="250" preview />
                </div>
            </div>
        <!-- <div>
            <pdf src="https://725920.selcdn.ru/upload_portkkm/iblock/618/618ced6fb507bce46330be90b2f799a6/PC_100-Instruktsiya-polzovatelya.pdf"></pdf>
        </div> -->
        </div>
        <div class="col-12 lg:col-12 xl:col-6">
            <div class="card">
                <h5>Перевод</h5>
                <div class="grid p-fluid mt-3">
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="lastname" />
                            <label for="inputtext">Фамилия</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="firstname" />
                            <label for="inputtext">Имя</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="middlename" />
                            <label for="inputtext">Отчество</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Dropdown id="dropdown" :options="sex" v-model="gender" optionLabel="name"></Dropdown>
                            <label for="dropdown">Пол</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Calendar inputId="calendar" v-model="birthDate"></Calendar>
                            <label for="calendar">Дата рождения</label>
                        </span>
                    </div>
                    <!-- <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <AutoComplete id="autocomplete" v-model="citizenship" :suggestions="filteredCountries" @complete="searchCountry($event)" field="name"></AutoComplete>
                            <label for="autocomplete">Гражданство</label>
                        </span>
                    </div> -->
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Dropdown id="selectedCountry" v-model="selectedCountry" :options="countries" optionLabel="name" placeholder="Гражданство" class="w-full md:w-14rem">
                                <template #value="slotProps">
                                    <div v-if="slotProps.value" class="flex align-items-center">
                                        <img :alt="slotProps.value.label" src="https://primefaces.org/cdn/primevue/images/flag/flag_placeholder.png" :class="`mr-2 flag flag-${slotProps.value.code.toLowerCase()}`" style="width: 18px" />
                                        <div>{{ slotProps.value.name }}</div>
                                    </div>
                                    <span v-else>
                                        {{ slotProps.placeholder }}
                                    </span>
                                </template>
                                <template #option="slotProps">
                                    <div class="flex align-items-center">
                                        <img :alt="slotProps.option.label" src="https://primefaces.org/cdn/primevue/images/flag/flag_placeholder.png" :class="`mr-2 flag flag-${slotProps.option.code.toLowerCase()}`" style="width: 18px" />
                                        <div>{{ slotProps.option.name }}</div>
                                    </div>
                                </template>
                            </Dropdown>
                            <label for="selectedCountry">Гражданство</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-12">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="birthPlace" />
                            <label for="inputtext">Место рождения</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-12">
                        <span class="p-float-label">
                            <Dropdown id="dropdown" :options="dul" v-model="valueDul" optionLabel="name"></Dropdown>
                            <label for="dropdown">Документ, удостоверяющий личность</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputText id="text" v-model="serNumDul"></InputText>
                            <label for="inputtext">Серия и номер</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="department"></InputText>
                            <label for="inputtext">Код подразделения</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Calendar inputId="calendar" v-model="dateIssue"></Calendar>
                            <label for="calendar">Дата выдачи</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-12">
                        <span class="p-float-label">
                            <InputText type="text" id="inputtext" v-model="issuedBy" />
                            <label for="inputtext">Кем выдан</label>
                        </span>
                    </div>
                    <!-- <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Chips inputId="chips" v-model="value4"></Chips>
                            <label for="chips">Chips</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputMask id="inputmask" mask="99/99/9999" v-model="value5"></InputMask>
                            <label for="inputmask">InputMask</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <InputNumber id="inputnumber" v-model="serNumDul"></InputNumber>
                            <label for="inputnumber">InputNumber</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <div class="p-inputgroup">
                            <span class="p-inputgroup-addon">
                                <i class="pi pi-user"></i>
                            </span>
                            <span class="p-float-label">
                                <InputText type="text" id="inputgroup" v-model="value7" />
                                <label for="inputgroup">InputGroup</label>
                            </span>
                        </div>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <Dropdown id="dropdown" :options="cities" v-model="gender" optionLabel="name"></Dropdown>
                            <label for="dropdown">Dropdown</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <MultiSelect id="multiselect" :options="cities" v-model="value9" optionLabel="name" :filter="false"></MultiSelect>
                            <label for="multiselect">MultiSelect</label>
                        </span>
                    </div>
                    <div class="field col-12 md:col-4">
                        <span class="p-float-label">
                            <MultiSelect id="multiselect" :options="cities" v-model="value9" optionLabel="name" :filter="false"></MultiSelect>
                            <label for="multiselect">MultiSelect</label>
                        </span>
                    </div> -->
                    <div class="field col-12 md:col-12">
                        <span class="p-float-label">
                            <Textarea inputId="textarea" rows="15" cols="30" v-model="textarea"></Textarea>
                            <label for="textarea">Дополнительно</label>
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style>
.p-image-preview-container > img {
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
}

.progress-spinner {
    position: fixed;
    z-index: 999;
    height: 2em;
    width: 2em;
    overflow: show;
    margin: auto;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
}

/* Transparent Overlay */
.progress-spinner:before {
    content: url('../../../public/demo/images/loader.gif');
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.53);
}
</style>
