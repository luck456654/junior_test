<template>
    <div class="page__center wide">
        <div class="page__row page__row_head" v-if="!mobile">
            <div class="page__col col__header d-flex flex-column flex-sm-row justify-content-space-between">
                <div class="mt-4 mt-sm-0">
                    <div class="page__hello h5" v-if="user" v-html="user.name+','"/>
                    <div class="page__welcome h2">Привет 👋</div>
                </div>

                <div class="mt-4 mt-sm-0">
                    <p class="mb-2">
                        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <circle class="color" cx="7" cy="7" r="6.5" fill="#7FBA7A"/>
                        </svg>

                        <span class="color-gray ml-2 align-middle">На продвижении</span>
                    </p>
                    <p>
                        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <circle class="color" cx="7" cy="7" r="6.5" fill="#FF4242"/>
                        </svg>

                        <span class="color-gray ml-2 align-middle">Есть заявки</span>
                    </p>
                </div>
            </div>
        </div>

        <div class="page__row" v-if="mobile">
            <div class="d-flex flex-row align-items-center justify-content-between">
                <h1>Мои треки</h1>
                <img :src="url + 'img/icon_plus_main.svg'" @click="openMusicModal" class="mb-2" />
            </div>
        </div>

        <div class="page__row" v-if="mobile">
             <div class="page__panel">
                <p>
                    <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle class="color" cx="6" cy="6" r="5.5" fill="#7FBA7A"/>
                    </svg>

                    <span>На продвижении</span>
                </p>
                <p>
                    <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle class="color" cx="6" cy="6" r="5.5" fill="#FF4242"/>
                    </svg>

                    <span>Есть заявки</span>
                </p>
            </div>
        </div>

        <div class="page__row page__row_border">
            <div class="page__col">
                <div class="products__grid">
                    <div class="products__item" @click="openMusicModal" v-if="!mobile">
                        <div class="products__preview new"></div>
                        <div class="products__details">
                            <div class="products__title title">Добавить трек</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <b-modal id="music-modal" centered hide-footer>
            <div class="modal-center d-flex flex-column text-center mx-auto">
                <div class="form-block">
                    <input type="text" class="form-control" placeholder="Ссылка на звук в TikTok" v-model="val" required="" />
                    <p class="form-tip text-danger" v-if="error" v-html="error" />
                    <button class="btn btn-lg btn-primary btn-block my-4" @click="getMusic(val)" :disabled="!val" v-if="!waiting" v-html="val ? 'Найти трек' : 'Введите ссылку на трек'" />
                    <div class="loading" :class="{active: waiting}" />
                    <p class="form-tip" v-if="waiting" v-html="'Ищем трек, это займет от 5 до 10 секунд'" />
                </div>
            </div>
        </b-modal>
    </div>
</template>

<script>
    import axios from "axios"
    import {mapGetters} from "vuex";
    import { GET_MUSIC_LIST, GET_MUSIC, ADD_MUSIC } from '../api-routes';

    export default {
        components: { },
        name: "Music",

        data() {
            return {
                editing: false,
                val: this.value,
                music: null,
                error: null,
                waiting: false,
                items: null,
            }
        },

        mounted() {
            this.getMusicList();
        },

        computed: {
            ...mapGetters(['user', 'userAuthorized', 'url', 'tablet', 'mobile']),
        },

        methods: {
            openMusicModal() {
                this.waiting = this.error = false;
                this.$bvModal.show('music-modal');
            },

            getMusicList() {
                axios.get(GET_MUSIC_LIST).then(response => {
                    this.items = response.data;
                });
            },

            getMusic(url) {
                this.waiting = true;
                this.error = null;

                axios.post(GET_MUSIC, {url: url}).then(response => {
                    this.waiting = false;

                    /* TO DO */
                    console.log(response);

                    this.error = null;
                }).catch(error => {
                    console.log(error);
                    this.waiting = false;
                    if(error !== undefined) {
                        this.error = 'Не удалось найти трек, попробуйте еще раз';
                    }
                });
            }
        }

    }
</script>
<template>
    <div class="page__center wide">
        <div class="page__row page__row_head" v-if="!mobile">
            <div class="page__col col__header d-flex flex-column flex-sm-row justify-content-space-between">
                <div class="mt-4 mt-sm-0">
                    <div class="page__hello h5" v-if="user" v-html="user.name+','"/>
                    <div class="page__welcome h2">Привет 👋</div>
                </div>

                <div class="mt-4 mt-sm-0">
                    <p class="mb-2">
                        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <circle class="color" cx="7" cy="7" r="6.5" fill="#7FBA7A"/>
                        </svg>

                        <span class="color-gray ml-2 align-middle">На продвижении</span>
                    </p>
                    <p>
                        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <circle class="color" cx="7" cy="7" r="6.5" fill="#FF4242"/>
                        </svg>

                        <span class="color-gray ml-2 align-middle">Есть заявки</span>
                    </p>
                </div>
            </div>
        </div>

        <div class="page__row" v-if="mobile">
            <div class="d-flex flex-row align-items-center justify-content-between">
                <h1>Мои треки</h1>
                <img :src="url + 'img/icon_plus_main.svg'" @click="openMusicModal" class="mb-2" />
            </div>
        </div>

        <div class="page__row" v-if="mobile">
             <div class="page__panel">
                <p>
                    <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle class="color" cx="6" cy="6" r="5.5" fill="#7FBA7A"/>
                    </svg>

                    <span>На продвижении</span>
                </p>
                <p>
                    <svg width="12" height="12" viewBox="0 0 12 12" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle class="color" cx="6" cy="6" r="5.5" fill="#FF4242"/>
                    </svg>

                    <span>Есть заявки</span>
                </p>
            </div>
        </div>

        <div class="page__row page__row_border">
            <div class="page__col">
            
			<!-- Баннер десктопная версия -->	
			<div class="banner" v-if="windowWidth>1024">
				<div class="texts-banner">
					<div class="banner-header">Загрузи свой первый трек</div>
					<div class="bannertext">Твои будущие фанаты ждут! Жми на кнопку «добавить трек» и переходи к продвижению прямо сейчас.</div>
					<a class="butksaton-satokavate" href="#clafolowing-dashows"><button class="add-track "><div class="img-button-addtrack">+</div>Добавить трэк</button></a>
                </div>
                <img :src="bannermain" class="bannerimg"></img>
            </div>	
				
			<!-- Баннер мобильная версия -->
			<div class="mbanner" v-if="windowWidth<321">
               <div class="mtexts-banner">
                  <div class="mbanner-header">Загрузи свой первый трек</div>
                  <div class="mbanner-text">Твои будущие фанаты ждут! Жми на кнопку «добавить трек» и переходи к продвижению прямо сейчас.</div>
                 <a class="butksaton-satokavate" href="#clafolowing-dashows"><button class="add-track add-track2"><div class="img-button-addtrack">+</div>Добавить трэк</button></a>
                  <div class="mimg-banner">
               <img src="/images/banner.png" class="mbannerimg"></img>
               </div>
            </div>
          </div>
            <!-- Баннер планшетная версия -->
			<div class="plbanner" v-if="((windowWidth<=1024)&(windowWidth>321))">
					<div class="pltexts-banner">
							<div class="plbanner-header">Загрузи свой первый трек</div>
							<div class="plbannertext">Твои будущие фанаты ждут! Жми на кнопку «добавить трек» и переходи к продвижению прямо сейчас.</div>
					<a class="butksaton-satokavate" href="#clafolowing-dashows"><button class="add-track add-track3"><div class="img-button-addtrack">+</div>Добавить трэк</button></a>
                 
					</div>
					<div class="plbanner">
					<img src="/images/banner.png" class="plbanner-img"></img>
				</div>
			</div>


			
				
			<!-- Модальное окно добавление трэка -->	
			<div class="anelumen lowingnuska" id="clafolowing-dashows"><a href="#/" class="nedismiseg"></a>
 
  <figure>
	  <h2><a href="#/" class="compatibg-ukastywise" aria-label="Close Modal Box" @click="add-track"><div class="img-close"><center>×</center></div></a></h2>
	  <center><img src="/images/avatar.png" class="avatar-style" ></img></center>
	  <center>
	  
	  <p class="style-label-first" >Cсылка на звук в TikTok</br></p>
		<input class="style-input2" placeholder="Cсылка на звук в TikTok" v-model="linktrak"></p></input>
	  <p class="style-label">Название</br></p>
		<input class="style-input2" placeholder="Название трека" v-model="nametrak"></p></input>
	  <p class="style-label" >Исполнитель</br></p>
		<input class="style-input2" placeholder="Исполнитель трека" v-model="solisttrak"></p></input>
	  <p class="style-label" >Альбом</br></p>
		<input class="style-input4" placeholder="Альбом трека" v-model="albumtrak"></p></input>
	 </center>
	 <center><button class="style-button2" @click="addedtrak">Добавить</button></center>
  </figure>
 
</div>	
				
			{{musicList.data}}	
				
				
            </div>
        </div>

        <b-modal id="music-modal" centered hide-footer>
            <div class="modal-center d-flex flex-column text-center mx-auto">
                <div class="form-block">
                    <input type="text" class="form-control" placeholder="Ссылка на звук в TikTok" v-model="val" required="" />
                    <p class="form-tip text-danger" v-if="error" v-html="error" />
                    <button class="btn btn-lg btn-primary btn-block my-4" @click="getMusic(val)" :disabled="!val" v-if="!waiting" v-html="val ? 'Найти трек' : 'Введите ссылку на трек'" />
                    <div class="loading" :class="{active: waiting}" />
                    <p class="form-tip" v-if="waiting" v-html="'Ищем трек, это займет от 5 до 10 секунд'" />
                </div>
            </div>
        </b-modal>
    </div>
</template>

<script>
    import axios from "axios"
    import {mapGetters} from "vuex";
    import { GET_MUSIC_LIST, GET_MUSIC, ADD_MUSIC } from '../api-routes';
    export default {
        components: { },
        name: "Music",
        data() {
            return {
                editing: false,
                val: this.value,
                music: null,
                error: null,
                waiting: false,
                items: null,
				windowWidth:window.screen.width,
				bannermain:"/images/banner.png",
				musicList:'',
				linktrak:'',
				nametrak:'',
				solisttrak:'',
				albumtrak:'',
            }
        },
		mounted() {
			this.createListMusic();
			
			this.getMusicList();
			window.addEventListener("resize",(event)=>{
			this.windowWidth=window.innerWidth;
			}) 
			
        },
        computed: {
            ...mapGetters(['user', 'userAuthorized', 'url', 'tablet', 'mobile']),
        },
		
        methods: {
		   openMusicModal() {
                this.waiting = this.error = false;
                this.$bvModal.show('music-modal');
            },
            getMusicList() {
                axios.get('/api/v1/music_list').then(response => {
                    this.items = response.data;
                });
            },
			
			createListMusic(){
			alert("Создаю список треков")
			axios.get('/api/v1/music_list').then(response => {
                    this.musicList = response.data;
					})
							 
           	},
			addedtrak(){
			try{
			axios.post(ADD_MUSIC,{
			url:this.linktrak,
			title:this.nametrak,
			author:this.solisttrak,
			album:this.albumtrak
			}).then(response => {
				    this.musicList = response.data;
					})
					.then(
					alert("Трэк успешно добавлен")
					)
					)
				}
			catch{
			(alert("Произошла ошибка не удалось добавить трек"))     
            }		
			},
			
			
            getMusic(url) {
                this.waiting = true;
                this.error = null;
                axios.post(GET_MUSIC, {url: url}).then(response => {
                    this.waiting = false;
                    /* TO DO */
                    console.log(response);
                    this.error = null;
                }).catch(error => {
                    console.log(error);
                    this.waiting = false;
                    if(error !== undefined) {
                        this.error = 'Не удалось найти трек, попробуйте еще раз';
                    }
                });
            }
        }
    }
	
</script>