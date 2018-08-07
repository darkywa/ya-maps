<template lang="pug">

    form(@submit.prevent="sendForm" class="distanceCitiesForm")
        fieldset(:disabled="disabled")
            div.row
                div(class="col-lg-4 col-md-4 col-sm-12")
                    div.form-group
                        input(
                            type="text"
                            required=""
                            class="form-control"
                            v-model="pointA"
                            id="point_a"
                            placeholder="Пункт А"
                        )
                div(class="col-lg-4 col-md-4 col-sm-12")
                    div.form-group
                        input(
                            type="text"
                            required=""
                            class="form-control"
                            v-model="pointB"
                            id="point_b"
                            placeholder="Пункт B"
                        )
                div(class="col-lg-4 col-md-4 col-sm-12")
                    div.form-group
                        button(type="submit" class="btn btn-primary") Отправить
                        button(type="reset" class="btn btn-danger") Очистить

</template>

<script>
export default {
    props: {
        disabled: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            pointA: '',
            pointB: '',
        }
    },
    methods: {
        sendForm(form) {
            // регистрируем событие для передачи данные наверх
            this.$emit('subdata', {
                form: form,
                pointA: this.pointA,
                pointB: this.pointB
            });
            this.pointA = this.pointB = '';
        }
    }
}
</script>

<style lang="less">
.distanceCitiesForm {
    margin: 0 auto 50px;

    button[type="submit"] {
        margin-right: 15px;
    }
}
</style>
