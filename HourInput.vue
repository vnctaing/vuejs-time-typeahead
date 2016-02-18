<style>
    .hour-input__container{
        position: relative;
    }
    .hour-input__item{
        list-style: none;
        cursor: pointer;
        padding: 10px;
        border-bottom: 1px solid #f2f2f2;
        border-radius: 3px;
    }

    .hour-input__item:hover{
        background-color: #36d7b7;
        color: #fff;
    }
    
    .hour-input__list{
        margin: 0;
        position: absolute;
        width: 100%;
        background-color: #fff;
        padding: 0 10px;
        z-index: 9999;
        border: 1px solid #f2f2f2;
    }

    .hour-input__input{
        width: 100%;
        height: 38px;
        padding-left: 13px;
        overflow: hidden;
        border-radius: 3px;
    }

    .hour-input_search-icon{
        position: absolute;
        right: 13px;
        top: 0;
        bottom: 0;
        margin: auto;
        height: 50%;
    }

</style>

<template>
    <div>
        <div class="hour-input__container">
            <div>
                <input type="text" v-model="hourinput" class="hour-input__input" placeholder="{{placeholder}}">
                <div class="hour-input_search-icon"><i class="fa fa-clock-o"></i></div>
            </div>
            <ul class="hour-input__list" v-if="suggestedHours.length">
                <li v-on:click="select(hour)" v-for="hour in suggestedHours" class="hour-input__item">{{hour}}</li>
            </ul>
        </div>
    </div>
</template>
<script>
    export default {
        props: ['placeholder'],
        data: () => {
            hourinput: ''
        },
        computed: {
            suggestedHours: function () {
                if (!isNaN(this.hourinput) && this.hourinput && this.hourinput <= 23){
                    const inputHour =  ("0" + this.hourinput).slice(-2);
                    let req = [];
                    const range = ['00','15','30','45'];
                    _.each(range, (r) => {
                        let str = '';
                        str = str.concat(inputHour, ':',r,':00');
                        req.push(str)
                    });
                    return req;                    
                } else {
                    return [];
                }

            }
        },
        methods: {
            select: function(hour){
                this.hourinput = hour;
            }

        },
        watch: {
            hourinput: function (){
                this.$dispatch('hour-output', this.suggestedHours);
            }
        }
    }
</script>