<template>
    <main class="main">
        <div class="container">
            <h1 class="main__heading">
                <p v-show="!isFormCorrect">Пожалуйста, заполните регистрационную форму</p>
                <p v-show="isFormCorrect">Регистрация успешно завершена!</p>
            </h1>

            <div class="main__form">
                <form action="#" class="form" novalidate>

                    <!-- Form 1 -->
                    <transition name="slide-fade"> 
                        <div v-show="step === 1" class="form__step">
                            <h3>Личные данные</h3>

                            <div class="form__validated">                           
                                <div class="form__item">
                                    <label for="surname">Фамилия*</label>
                                    <input 
                                        type="text" 
                                        id="surname" 
                                        placeholder="Иванов" 
                                        @input="nonDigitMask()" 
                                        v-model.trim="personalData.surname"
                                        @blur="$v.personalData.surname.$touch()"    
                                        :class="{'invalid': $v.personalData.surname.$error}"    
                                    >
                                </div>
                                <span class="invalid-feedback" v-if="$v.personalData.surname.$error">{{ requiredText }}</span>
                            </div>
                            
                            <div class="form__validated"> 
                                <div class="form__item">
                                    <label for="name">Имя*</label>
                                    <input 
                                        type="text" 
                                        id="name" 
                                        placeholder="Иван" 
                                        @input="nonDigitMask()" 
                                        v-model.trim="personalData.name"
                                        @blur="$v.personalData.name.$touch()"
                                        :class="{'invalid': $v.personalData.name.$error}"    
                                    >
                                </div>       
                                <span class="invalid-feedback" v-if="$v.personalData.name.$error">{{ requiredText }}</span>
                            </div>              

                            <div class="form__item"> 
                                <label for="lastname">Отчество</label>
                                <input 
                                    type="text" 
                                    id="lastname" 
                                    placeholder="Иванович" 
                                    v-model="personalData.secondName"
                                    @input="nonDigitMask()" 
                                >
                            </div>

                            <div class="form__validated"> 
                                <div class="form__item">
                                    <label for="birthday">Дата рождения*</label>
                                    <input 
                                        type="text" 
                                        id="birthday" 
                                        placeholder="26.12.1991" 
                                        v-model="personalData.birthday"
                                        @blur="$v.personalData.birthday.$touch()"
                                        @input="dateMask()"
                                        :class="{'invalid': $v.personalData.birthday.$error}"  
                                        >
                                </div>
                                <span class="invalid-feedback" v-if="$v.personalData.birthday.$error">{{ requiredText }}</span>
                            </div>       

                            <div class="form__validated"> 
                                <div class="form__item">
                                    <label for="tel">Номер телефона*</label>
                                    <input 
                                        type="tel" 
                                        id="tel" 
                                        placeholder="79998998989" 
                                        @input="phoneMask" 
                                        v-model="personalData.phone"
                                        @blur="$v.personalData.phone.$touch()"
                                        :class="{'invalid': $v.personalData.phone.$error}"
                                    >    
                                </div>
                                <span class="invalid-feedback" v-if="$v.personalData.phone.$error">{{ requiredText }}</span>
                                <span class="invalid-feedback" v-if="$v.personalData.phone.$invalid && $v.personalData.phone.$error">{{ invalidPhone }}</span>
                            </div>       

                               
                            <div class="form__item form__radio"> 
                                <label for="sex">Пол</label>
                                <div class="form__radio-group">
                                    <label>
                                        <input type="radio" name="radio" value="Мужчина" v-model="personalData.sex">
                                        <div></div>
                                        <span>Мужчина</span>
                                    </label>
                                    <label>
                                        <input type="radio" name="radio" value="Женщина" v-model="personalData.sex">
                                        <div></div>
                                        <span>Женщина</span>
                                    </label>
                                </div>
                            </div>

                            <div class="form__validated"> 
                                <multiselect 
                                    class="multiselect" 
                                    v-model="personalData.clientType" 
                                    :options="multiselect.options1" 
                                    :multiple="true"
                                    :placeholder="multiselect.placeholder1"
                                    @close="$v.personalData.clientType.$touch()"
                                    :class="{'invalid': $v.personalData.clientType.$error}"
                                ></multiselect>                               
                                <span class="invalid-feedback extra-space" v-if="$v.personalData.clientType.$error">{{ requiredText }}</span>
                            </div>

                            <div class="form__validated"> 
                                <multiselect2 
                                    class="multiselect" 
                                    v-model="personalData.doctor" 
                                    :options="multiselect.options2" 
                                    :placeholder="multiselect.placeholder2"
                                ></multiselect2>                               
                            </div>


                            <div class="form__item form__checkbox"> 
                                <span class="text-cb">Не отправлять СМС</span> 
                                <label>
                                    <input type="checkbox" class="cb" v-model="personalData.sendSMS">       
                                    <span></span>      
                                </label>
                            </div>

                            <div class="form__item">
                                <p class="form__required">{{requiredText}}</p>
                            </div>


                            <button class="nextBtn" @click.prevent="checkValidation1">
                                Следующий шаг
                            </button>
                            
                        </div>
                    </transition> 


                    <!-- Form 2 -->
                    <transition name="slide-fade"> 
                        <div v-show="step === 2" class="form__step">
                        <h3>Адрес</h3>

                        <div class="form__item">
                            <label for="surname">Индекс</label>
                            <input 
                                type="text" 
                                id="surname" 
                                placeholder="123456" 
                                v-model="personalData.index"
                                @input="nonLetterMask()"
                            >
                        </div>

                        <div class="form__item">
                            <label for="surname">Страна</label>
                            <input 
                                type="text" 
                                id="surname" 
                                placeholder="Россия" 
                                v-model="personalData.country"
                                @input="nonDigitMask()"
                            >
                        </div>

                        <div class="form__item">
                            <label for="surname">Область</label>
                            <input type="text" id="surname" placeholder="Тульская область" v-model="personalData.region">
                        </div>

                        <div class="form__validated">                           
                            <div class="form__item">
                                <label for="city">Город*</label>
                                <input 
                                    type="text" 
                                    id="city" 
                                    placeholder="Тула" 
                                    @input="nonDigitMask()"
                                    v-model.trim="personalData.city"
                                    @blur="$v.personalData.surname.$touch()"    
                                    :class="{'invalid': $v.personalData.city.$error}"    
                                >
                            </div>
                            <span class="invalid-feedback" v-if="$v.personalData.city.$error">{{ requiredText }}</span>
                        </div>

                        <div class="form__item">
                            <label for="surname">Улица</label>
                            <input 
                                type="text" 
                                id="surname" 
                                placeholder="ул. Солнечная" 
                                v-model="personalData.street"
                                @input="nonDigitMask()"
                            >
                        </div>

                        <div class="form__item">
                            <label for="surname">Дом</label>
                            <input type="text" id="surname" placeholder="45к2" v-model="personalData.building">
                        </div>

                    
                        <div class="form__item">
                            <p class="form__required">{{requiredText}}</p>
                        </div>

                        <div class="form__btn-group">
                            <button class="prevBtn" @click.prevent="prevStep">
                                Назад
                            </button>
                            <button class="nextBtn" @click.prevent="checkValidation2">
                                Следующий шаг
                            </button>
                        </div>

                        </div>
                    </transition>
            
                    <!-- Form 3 -->
                    <transition name="slide-fade"> 
                        <div v-show="step === 3" class="form__step">
                            <h3>Документ</h3>

                            <div class="form__validated"> 
                                <multiselect3 
                                    class="multiselect" 
                                    v-model="personalData.document" 
                                    :options="multiselect.options3" 
                                    :placeholder="multiselect.placeholder3"
                                    @close="$v.personalData.document.$touch()"
                                    :class="{'invalid': $v.personalData.document.$error}"
                                ></multiselect3>                               
                                <span class="invalid-feedback extra-space" v-if="$v.personalData.document.$error">{{ requiredText }}</span>
                            </div>

                            <div class="form__item">
                                <label for="serial">Серия</label>
                                <input 
                                    type="text" 
                                    id="serial" 
                                    placeholder="1234" 
                                    v-model="personalData.serial" 
                                    @input="serialMask()">
                            </div>

                            <div class="form__item">
                                <label for="surname">Номер</label>
                                <input type="text" id="given" placeholder="123456" v-model="personalData.passNum" @input="nonLetterMask()">
                            </div>

                            <div class="form__item">
                                <label for="given">Кем выдан</label>
                                <input type="text" id="surname" placeholder='ОВД "Гольяново" гор. Москвы' v-model="personalData.givenBy">
                            </div>
    
                            <div class="form__validated">
                                <div class="form__item">
                                    <label for="obtain">Дата выдачи*</label>
                                    <input 
                                        type="text" 
                                        id="obtain" 
                                        placeholder="27.12.1991" 
                                        v-model="personalData.obtainingDate"
                                        @blur="$v.personalData.obtainingDate.$touch()"
                                        @input="dateMask"
                                        :class="{'invalid': $v.personalData.obtainingDate.$error}"
                                    >
                                </div>
                                <span class="invalid-feedback" v-if="$v.personalData.obtainingDate.$error">{{ requiredText }}</span>
                            </div>

                            
                            <div class="form__item">
                                <p class="form__required">{{requiredText}}</p>
                            </div>


                            <div class="form__btn-group">
                                <button class="prevBtn" @click.prevent="prevStep">
                                    Назад
                                </button>
                                <button class="nextBtn" @click.prevent="checkValidation3">
                                    Регистрация
                                </button>
                            </div>

                        </div>
                    </transition>


                </form>
            </div>
        </div>

        <Modal
            v-if="isModalShowed"
            @closeModal="closeModal"
            :formData="formData"
            @submitForm="submitForm()"
        />

    </main>
</template>


<script>
import { required, minLength } from 'vuelidate/lib/validators'
import Multiselect from 'vue-multiselect'
import Multiselect2 from 'vue-multiselect'
import Multiselect3 from 'vue-multiselect'
import Modal from './Modal'

export default {
    name: 'Form',
    components: {
        Multiselect,
        Multiselect2,
        Multiselect3,
        Modal
    },
    data() {
        return {
            multiselect: {
                placeholder1: 'Группа клиентов*',
                options1: ['VIP','Проблемные', 'ОМС'],
                placeholder2: 'Лечащий врач',
                options2: ['Иванов', 'Захаров', 'Чернышева'],
                placeholder3: 'Тип документа*',
                options3: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
            },
            isModalShowed: false,
            step: 1,
            isFormCorrect: false,
            requiredText: '*Поле обязательно для заполнения',
            invalidPhone: '*Минимальная длина 11 цифр!',
            formData: [],
            personalData: {
                index: '',
                country: '',
                region: '',
                city: '',
                street: '',
                building: '',
                name: '',
                surname: '',
                secondName: '',
                phone: '',
                birthday: '',
                sex: '',
                clientType: '',
                doctor: '',
                sendSMS: false,
                document: '',
                serial: '',
                passNum: '',
                givenBy: '',
                obtainingDate: ''
            }
        }
    },
    validations: {
        personalData: {
            name: {
                required
            },
            surname: {
                required
            },
            birthday: {
                required
            },
            phone: {
                required,
                minLength: minLength(11)
            },
            clientType: {
                required
            },
            city: {
                required
            },
            document: {
                required
            },
            obtainingDate: {
                required
            }

        }
    },
    methods: {
        submitForm() {
            this.isFormCorrect = true
            this.step = 0
        },
        checkValidation1() {
            if (
                !this.$v.personalData.name.$dirty ||
                !this.$v.personalData.surname.$dirty ||
                !this.$v.personalData.phone.$dirty ||
                !this.$v.personalData.clientType.$dirty ||
                !this.$v.personalData.birthday.$dirty
            ) {
                this.$v.personalData.name.$touch()
                this.$v.personalData.surname.$touch()
                this.$v.personalData.phone.$touch()
                this.$v.personalData.clientType.$touch()
                this.$v.personalData.birthday.$touch()
            } else if (
                !this.$v.personalData.name.$error &&
                !this.$v.personalData.surname.$error &&
                !this.$v.personalData.phone.$error &&
                !this.$v.personalData.clientType.$error &&
                !this.$v.personalData.birthday.$error
            ) {
                this.nextStep()
            }
        },
        checkValidation2() {
            if (
                !this.$v.personalData.city.$dirty
            ) {
                this.$v.personalData.city.$touch()
            } else if (
                !this.$v.personalData.city.$error
            ) {
                this.nextStep()
            }
        },
        checkValidation3() {
            if (
                !this.$v.personalData.document.$dirty ||
                !this.$v.personalData.obtainingDate.$dirty
            ) {
                this.$v.personalData.document.$touch()
                this.$v.personalData.obtainingDate.$touch()
            } else if (
                !this.$v.personalData.document.$error &&
                !this.$v.personalData.obtainingDate.$error
            ){
                this.showModal()
                this.check()
            }
        },
        showModal() {
            this.isModalShowed = true
        },
        closeModal() {
            this.isModalShowed = false
        },
        serialMask() {
            this.personalData.serial = this.personalData.serial.replace(/\D+/gi, '')
            const serial = this.personalData.serial.match(/\d{4}/g)
            this.personalData.serial = this.personalData.serial.replace(/\d{4,}/g, serial).replace(/((\d{2})(\d{2}))/g, `$2 $3`)
        },
        phoneMask() {
            this.personalData.phone = this.personalData.phone.replace(/\D+/gi, '')

            const tel = this.personalData.phone.match(/\d{11}/g)
            this.personalData.phone = this.personalData.phone.replace(/\d{11,}/g, tel)
            let mask = ''
            if (tel) {
                const num = tel[0]
                const operator = num[1]+num[2]+num[3]
                const part1 = num[4]+num[5]+num[6]
                const part2 = num[7]+num[8]
                const part3 =  num[9]+num[10]
                mask = `+7 (${operator}) ${part1}-${part2}-${part3}`
            }
            if (tel) this.personalData.phone = mask
        },
        nonDigitMask() {
            this.personalData.surname     = this.personalData.surname.replace(/\d+/g, '')
            this.personalData.name        = this.personalData.name.replace(/\d+/g, '')
            this.personalData.secondName  = this.personalData.secondName.replace(/\d+/g, '')
            this.personalData.country     = this.personalData.country.replace(/\d+/g, '')
            this.personalData.region      = this.personalData.region.replace(/\d+/g, '')
            this.personalData.city        = this.personalData.city.replace(/\d+/g, '')
        },
        dateMask() {           
            this.personalData.birthday      = this.personalData.birthday.replace(/[^0-9/]+/gi, '')
            this.personalData.obtainingDate = this.personalData.obtainingDate.replace(/[^0-9/]+/gi, '')

            const Date_1 = this.personalData.birthday.match(/\d{8}/g)
            this.personalData.birthday = this.personalData.birthday.replace(/\d{8,}/g, Date_1)
            let DateMask1 = ''
            if (Date_1) {
                const thedate = Date_1[0]
                const day = thedate[0]+thedate[1]
                const month = thedate[2]+thedate[3]
                const year = thedate[4]+thedate[5] +thedate[6]+thedate[7]
                DateMask1 = `${day}.${month}.${year}`
            }
            if (Date_1) this.personalData.birthday = DateMask1

            const Date_2 = this.personalData.obtainingDate.match(/\d{8}/g)
            this.personalData.obtainingDate = this.personalData.obtainingDate.replace(/\d{8,}/g, Date_2)
            let DateMask2 = ''
            if (Date_2) {
                const thedate = Date_2[0]
                const day = thedate[0]+thedate[1]
                const month = thedate[2]+thedate[3]
                const year = thedate[4]+thedate[5] +thedate[6]+thedate[7]
                DateMask2 = `${day}.${month}.${year}`
            }
            if (Date_2) this.personalData.obtainingDate = DateMask2

            
        },
        nonLetterMask() {
            this.personalData.index = this.personalData.index.replace(/\D/gi, '')
            this.personalData.serial = this.personalData.serial.replace(/\D/gi, '')
            this.personalData.passNum = this.personalData.passNum.replace(/\D/gi, '')

            const index = this.personalData.index.match(/\d{6}/g)
            this.personalData.index = this.personalData.index.replace(/\d{6,}/g, index)

            const passNum = this.personalData.passNum.match(/\d{6}/g)
            this.personalData.passNum = this.personalData.passNum.replace(/\d{6,}/g, passNum)
        },
        nextStep() {
            this.step++
        },
        prevStep() {
            this.step--
        },
        check() {
            const name = `ФИО: ${this.personalData.surname} ${this.personalData.name} ${this.personalData.secondName}`.trim()

            const address = `
            Адрес: ${this.personalData.city + ','} ${this.personalData.country + ','} ${this.personalData.region + ','} ${this.personalData.street + ','} дом ${this.personalData.building + ','} ${this.personalData.index + ','}` 
                .replace(/,\s,|дом\s,|индекс\s,|\s,\s/g, '')
                .trim()
                .replace(/,$/g, '')

            const sex        = this.personalData.sex ? `Пол: ${this.personalData.sex}` : ''
            const birthday   = this.personalData.birthday? `Дата рождения: ${this.personalData.birthday}` : ''
            const doctor     = this.personalData.doctor ? `Лечащий врач: ${this.personalData.doctor}` : ''

            const phone      = `Контактные данные: ${this.personalData.phone}`
            const clientType = `Группа клиентов: ${this.personalData.clientType}` 


            const sendSMS = this.personalData.sendSMS ? `СМС: не присылать` : `СМС: присылать`

            
            const document = `Документ: ${this.personalData.document}`
            const obtDate  = `Дата выдачи: ${this.personalData.obtainingDate}`

            const serial   = this.personalData.serial && this.personalData.passNum ? `Серия и номер: ${this.personalData.serial} ${this.personalData.passNum}` : ''
            const givenBy  = this.personalData.givenBy ? `Выдан: ${this.personalData.givenBy}` : ''
                        
            this.formData = Object.entries({
                name,
                sex,
                birthday,
                clientType,
                doctor,
                phone,
                sendSMS,
                address,
                document,
                serial,
                givenBy,
                obtDate
            })
        }
    }
}
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>

<style lang="sass" scoped>
@import '../sass/vars'

.main
    h1
        margin-top: 30px
        font-size: 1rem
.form
    &__required
        font-size: 0.7rem
    &__validated
        .multiselect
            margin: 0 auto
            width: 380px
            transition: 0.2s
        margin-bottom: 15px
        > span
            display: block
            text-align: right
            margin-right: 25px
            font-size: 0.7rem
    &__btn-group
        display: flex
    &__step
        position: relative
        button
            cursor: pointer
            padding: 10px
            display: block
            margin: 20px 24px 0 auto   
            border-radius: 8px  
            transition: .2s 
            &.nextBtn
                color: #ffffff
                border: none 
                margin: 20px 24px 0 auto    
                background: $main-color
                &:hover
                    background: lighten($main-color, 5%)
            &.prevBtn
                background: #fff
                color: $main-color
                margin: 20px auto 0 24px
                border: 1px solid $main-color
                &:hover
                    background: $main-color
                    color: #fff
        width: 450px
        margin-top: 40px
        box-sizing: border-box
        padding: 15px 10px 15px 10px
        border: 1px solid #ebebeb
        border-radius: 8px
        box-shadow: 2px 2px 2px 0px rgba(0, 0, 0, 0.2)
        h3  
            text-align: center
            border-bottom: 1px solid #ebebeb
            padding: 0 0 15px 0
            margin-bottom: 25px
            font-size: 1.2rem
    &__list
        margin: 15px auto 20px
        width: 380px
        display: flex
        justify-content: center
        p
            color: $main-color
            margin-right: 15px
    &__item
        margin: 0 auto
        width: 380px
        display: flex
        justify-content: space-between
        align-items: center
        margin-bottom: 15px
        input[type=text], input[type=tel]
            box-sizing: border-box
            padding: 0 0 0 10px
            transition: .2s
            height: 35px
            width: 200px
            border: 1px solid #ebebeb
            border-radius: 8px
            &:focus
                border: 2px solid $main-color
        select
            box-sizing: border-box
            padding: 0px 10px 0px 10px
            width: 200px
            height: 35px
            border: 2px solid #ebebeb
            border-radius: 8px
            &[multiple]
                padding: 5px 0 0 10px
                height: 100%      
    &__checkbox
        display: flex
        justify-content: space-between
        align-items: center
        width: 195px
        margin-top: 25px
        label
            span
                position: relative
                display: inline-block
                width: 20px
                height: 20px
                border: 1px solid #ebebeb
                border-radius: 4px
                &::before
                    content: '\2714'
                    color: $main-color
                    position: absolute
                    top: 50%
                    left: 50%
                    transform: translate(-40%, -50%)
                    display: block
                    width: 15px
                    opacity: 0
                    transition: 0.2s
            input
                display: none
                &:checked + span::before
                    opacity: 1
    &__radio
        padding: 5px 0
        user-select: none
        &-group
            display: flex
            justify-content: space-between
            align-items: center
            width: 200px
            label
                display: flex
                justify-content: space-between
                align-items: center
                div
                    position: relative
                    display: inline-block
                    width: 16px
                    height: 16px
                    border: 1px solid #ebebeb
                    border-radius: 50%
                    margin-right: 5px
                    &::before
                        content: ''
                        position: absolute
                        top: 50%
                        left: 50%
                        transform: translate(-50%, -50%)
                        display: block
                        width: 8px
                        height: 8px
                        border-radius: 50%
                        background-color: $main-color
                        opacity: 0
                        transition: 0.2s
                input
                    display: none
                    &:checked + div::before
                        opacity: 1

.invalid 
    border: 2px solid #c60304 !important
    border-radius: 8px
    &-feedback
        line-height: 1.3
        font-size: 0.8rem
        color: #c60304 
.extra-space
    margin-top: 10px   

@import '../sass/_media'
</style>