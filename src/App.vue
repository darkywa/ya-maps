<template lang="pug">

	div#app
		section.app-form
			div(id="appFormContainer" class="container-app")
				AddNewForm(@newform="addNewForm")
				template(v-for="n in formsCount")
					BaseForm(:disabled="isFormDisabled" @subdata="addNewLog")

		section.app-logs
			div.container-app
				div.row
					div.col
						h2.app-logs__title Журнал запросов:
						TodoList(:todos="todos")

</template>

<script>
import BaseForm from './components/BaseForm.vue'
import TodoList from './components/TodoList.vue'
import AddNewForm from './components/AddNewForm.vue'

export default {
	components: {
		BaseForm, TodoList, AddNewForm
	},
	data () {
        return {
            pointA: '',
            pointB: '',
            todos: [],
            isFormDisabled: false,
            formsCount: 1
        }
    },
    methods: {
        // обращение к Я API и получение ответа в виде числа (км)
        getDistance: function() {

            var pointA  = this.pointA,
                pointB  = this.pointB;

            return new Promise(function(resolve, reject) {
                // Рассчитаем расстояние между точкой А и Б
                // Координаты точки А
                ymaps.geocode(pointA).then(function (res) {
                    var pointACoords = res.geoObjects.get(0).geometry.getCoordinates();
                    // Координаты точки Б
                    ymaps.geocode(pointB).then(function (res) {
                        var pointBCoords = res.geoObjects.get(0).geometry.getCoordinates(),
                        // Расстояние в км
                        response = Math.round(ymaps.coordSystem.geo.getDistance(pointACoords, pointBCoords) / 1000);

                        // возвращаем результат
                        resolve(response);
                    })
                    .catch(function(error) {
                        // выводим ошибку в консоль
                        console.log(error);

                        // возвращаем результат
                        reject(error);
                    });
                })
                .catch(function(error) {
                    // выводим ошибку в консоль
                    console.log(error);

                    // возвращаем результат
                    reject(error);
                });
            })
        },
        // добавление нового запроса
        addNewLog: function(form) {

            // блокируем форму
            this.isFormDisabled = true;

            this.pointA  = form.pointA; // поле А
            this.pointB  = form.pointB; // поле Б

            var form    = form.form, // форма
                todos   = this.todos, // массив элементов
                d       = new Date(),
                timeNow = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds(), // время
                // объект для ответа
                data    = {
                    dayNow: d.getDate(), // день
                    monthNow: d.getMonth() + 1, // месяц
                    timeNow: timeNow,
                    pointA: this.pointA,
                    pointB: this.pointB,
                },
                finallyForm = this.finallyForm, // метод для выполнения операций после запроса
                result  = this.getDistance(); // результат в км

            result.then(function(response) {
                // формируем ответ
                data.result = response;
                data.status = 'success';

                // добавляем в массив элементов
                todos.push(data);
            })
            .then(function() {
            	// сбрасываем данные
            	finallyForm(form);
            })
            .catch(function(error) {
                // формируем ответ
                data.result = error;
                data.status = 'error';

                // добавляем в массив элементов
                todos.push(data);

                // сбрасываем данные
            	finallyForm(form);
            })
        },
        // разблокируем и очищаем форму
        finallyForm: function(form) {
            // разблокируем форму
            this.isFormDisabled = false;

            // чистим поля
            this.pointA = this.pointB = '';

            // сбрасываем стили формы
            form.target.reset();
        },
        // добавление экземпляра формы, если форм < 10
        addNewForm: function() {
        	if (this.formsCount < 9) {
        		this.formsCount++
        	}
        }
    }
}
</script>

<style lang="less">
	body {
		background-color: #f5f5f5;
	}
	.container-app {
	    max-width: 850px;
	    position: relative;
	    margin-left: auto;
	    margin-right: auto;
	    padding-right: 15px;
	    padding-left: 15px;
	}

	#app {
		padding: 100px 0;
	}
</style>