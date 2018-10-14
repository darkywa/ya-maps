<template>
    <div id="app">
        <div>
            <select v-model="parentSelected" @change="getValuesBySelected" multiple="">
                <option v-for="item in parentList" :key="item.id" :value="item">
                    {{ item.name }}
                </option>
            </select>
            <pre>{{ groupConditionsName }}</pre>
        </div>
        <div>
            <multiselect
                v-model="conditionsFinall"
                :options="groupConditionsName"
                :multiple="true"
                :close-on-select="false"
                :clear-on-select="false"
                :preserve-search="true"
                group-values="values"
                group-label="parentName"
                :group-select="true"
                placeholder="Type to search"
                track-by="name"
                label="name"
            >
                <template slot="selection" slot-scope="{ values, search, isOpen }">
                    <span class="multiselect__single" v-if="values.length && !isOpen">
                        {{ values.length }} условий выбрано
                    </span>
                </template>
                <span slot="noResult">
                    Oops! No elements found. Consider changing the search query.
                </span>
            </multiselect>
            <pre>{{ conditionsFinall }}</pre>
        </div>
        <div>
            <button @click="saveAll">Сохранить</button>
        </div>
        <div>
            <div v-for="item in selectedName" :key="item.parentID">
                <div>
                    <b>{{item.parentName}}</b>
                </div>
                <div>
                    <div v-for="value in item.values" :key="value.id">
                        {{value.name}}
                    </div>
                </div>
                <hr>
            </div>
        </div>
    </div>
</template>

<script>

import _ from 'lodash'

export default {
    data() {
        return {
            /** хранилище для выбраных имен значений */
            parentSelected: [],
            parentList: [
                {id: 1, name: 'Температура'},
                {id: 2, name: 'Надрез'},
                {id: 3, name: 'Нагрузка'},
            ],
            childrenList: [
                {id: 1, author: 357, parent: 1, value: '10 C'},
                {id: 2, author: 357, parent: 1, value: '20 C'},
                {id: 3, author: 357, parent: 1, value: '30 C'},
                {id: 4, author: 357, parent: 1, value: '40 C'},
                {id: 5, author: 357, parent: 2, value: 'С надрезом'},
                {id: 6, author: 357, parent: 2, value: 'Без надреза'},
                {id: 7, author: 357, parent: 3, value: '10 кг'},
                {id: 8, author: 357, parent: 3, value: '20 кг'},
            ],
            /** группировка значений для мультиселекта */
            groupConditionsName: [],
            /** массив с выбранными значениями условий */
            conditionsFinall: [],
            /** итоговый результат с объектами выбранных значений и группировкой */
            selectedName: [],
        }
    },
    methods: {
        /**
         *  получение значений условий по выбраному ИД
         * 
         * @returns {void} добалвяет элементы groupConditionsName
         */
        getValuesBySelected() {
            // обновляем массив
            this.groupConditionsName = []
            // обходим в цикле выбранные имена условий и формируем группировку
            for (let i = 0; i < this.parentSelected.length; i++) {
                let myObj = {}
                myObj.parentID = this.parentSelected[i].id
                myObj.parentName = this.parentSelected[i].name
                myObj.values = []
                this.groupConditionsName.push(myObj)
            }
            // обходим в цикле значения условий, сравниваем их к какой группе имен они принадлежат
            let arr = this.childrenList
            for (let i = 0; i < arr.length; i++) {
                let obj = {id: arr[i].id, name: arr[i].value}
                this.groupConditionsName.forEach(element => {
                    if (element.parentID === arr[i].parent) {
                        obj.parentID = element.parentID
                        obj.parentName = element.parentName
                        element.values.push(obj)
                    }
                })
            }
        },
        /**
         *  формирование итогового массива с объектами имен и значений условия
         * 
         * @returns {void} фильтрует и добалвяет элементы в selectedName
         * @returns {void} добавляет сгруппированные элементы в selectedName
         */
        saveAll() {
            this.selectedName = []
            let arr = []
            // обходим в цикле массив с выбранными значениями условий
            // и выбираем из них имена условий для уникализации
            for (var i = 0; i < this.conditionsFinall.length; i++) {
                let obj = {
                    parentID: this.conditionsFinall[i].parentID,
                    parentName: this.conditionsFinall[i].parentName
                }
                arr.push(obj)
            }
            // записываем только имена условий, исходя из значений, которые были выбраны
            this.selectedName = _.uniqBy(arr, 'parentID')
            let arr2 = this.conditionsFinall
            // обходим в цикле массив с выбранными значениями условий
            // в кажду группу записываем значений выбранных условий
            for (var i = 0; i < arr2.length; i++) {
                let obj = {id: arr2[i].id, name: arr2[i].name}
                this.selectedName.forEach(element => {
                    if (!element.values) {
                        element.values = []
                    }
                    if (element.parentID === arr2[i].parentID) {
                        element.values.push(obj)
                    }
                })
            }
        },
    }
}
</script>

<style>
#app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      color: #2c3e50;
      margin-top: 60px;
      display: flex;
}
#app div {
    padding: 20px;
}
.conditions-item {
    display: flex;
}
.conditions-item__label {
    margin-right: 20px;
}
select {
    min-width: 100px;
}
pre {
    display: block;
    margin: 20px 0;
}
</style>
