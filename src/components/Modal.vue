<template>
    <div class="modal">

        <div class="modal__content">
            <span class="modal__close" @click="closeModal">&#10006;</span>
            <div class="modal__header">
                <h3>Регистрация</h3>
            </div>
            <h2>Все верно?</h2>
            <div class="modal__list">
                <div v-for="(data, index) of formData" :key="index">
                    {{data[1]}}
                </div>
            </div>
            <div class="modal__footer">
                <button class="btn-exit" @click="closeModal">Назад</button>
                <button class="btn-register" @click="submitForm">Да, все правильно</button>
            </div>        
        </div>

    </div>  
</template>

<script>
export default {
    name: 'modal',
    props: {
        formData: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            isFormCorrect: false,
        }
    },
    methods: {
        closeModal() {
            this.$emit('closeModal')
        },
        submitForm() {
            this.closeModal()
            this.isFormCorrect = true
            this.$emit('submitForm', this.isFormCorrect)
        }

    }
}
</script>

<style lang="sass" scoped>
@import '../sass/vars'

.modal
    position: fixed 
    left: 0
    top: 0
    width: 100%
    height: 100%
    z-index: 9
    background-color: rgba(0,0,0,0.5)
    transition: .2s
    h2
        font-size: 1.2rem
        margin-bottom: 20px
        text-align: left
        padding-left: 25px
        font-weight: bold
    &__list
        box-sizing: border-box
        text-align: left
        line-height: 1.8
        padding: 0px 25px
    &__content
        box-sizing: border-box
        width: 500px
        background: #fff
        // box-shadow: 0 0 17px 0 #e7e7e7 //neon effect
        border-radius: 8px
        position: fixed
        left: 50%
        top: 10%
        transform: translateX(-50%)
        padding: 20px
        text-align: center
    &__close
        cursor: pointer
        position: absolute
        right: 10px
        top: 8px
        transform: scale(1.3)
    &__header
        h3
            text-align: center
            display: inline
            border-bottom: 1px solid #ebebeb
            padding: 0 0 15px 0
            margin-bottom: 25px
            font-size: 1.2rem
            width: 100%
    &__header, &__footer   
        display: flex   
        justify-content: space-between
        align-items: center
    
    button
        cursor: pointer
        padding: 10px
        display: block
        margin: 20px 24px 0 auto   
        border-radius: 8px  
        transition: .2s 
        &.btn-register
            color: #ffffff
            border: none 
            margin: 20px 24px 0 auto    
            background: $main-color
            &:hover
                background: lighten($main-color, 5%)
        &.btn-exit
            background: #fff
            color: $main-color
            margin: 20px auto 0 24px
            border: 1px solid $main-color
            &:hover
                background: $main-color
                color: #fff

@import '../sass/_media'
</style>