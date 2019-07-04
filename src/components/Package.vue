<template>
    <div>
        <div class="wrap">
            <div class="col col-job">
                <p>{{title.numberJob + ' ' + item.name.slice(4)}}</p>
                <input type="number" min="0" class="input-field" v-model="countPackage">
            </div>
            <div class="col col-pre-count">
                <p>{{title.preCount}}</p>
                <p class="input-field">{{item.price}}</p>
            </div>
            <div class="col col-price">
                <p>{{title.price + ' gói ' + namePackage}}</p>
                <p class="input-field abc" v-model="modelPrice">{{packagePrice}}</p>
            </div>
            <div class="col col-date">
                <p>{{title.date + ' gói ' + namePackage}}</p>
                <input type="date" class="input-field" :value="formatDate(startDate, item.duration)">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Package',
        props: [
            'packageObj',
            'namePackage',
            'choosePackage',
            'item',
            'index'
        ],
        data() {
            return {
                title: {
                    'numberJob' : 'Số job',
                    'preCount'  : 'Tiền dự tính',
                    'price'     : 'Tổng tiền thực tế',
                    'date'      : 'Ngày hết hạn'
                },
                countPackage    : 1,
                startDate       : new Date(),
                modelPrice      : [],
            }
        },
        computed: {
            packagePrice() {
                return this.countPackage * this.item.price;
            },
        },
        watch: {
            packagePrice() {
                console.log(this.packagePrice)
            },
        },
        methods: {
            formatDate: function (date, duration) {
                let newDate = new Date();
                newDate.setDate(date.getDate()+parseInt(duration));
                let year = newDate.getFullYear();
                let month = (1 + newDate.getMonth()).toString();
                month = month.length > 1 ? month : '0' + month;
                let day = newDate.getDate().toString();
                day = day.length > 1 ? day : '0' + day;
                return year + '-' + month + '-' + day;
            }
        }
    }
</script>
<style scoped>
    .wrap {
        padding-left: 17%;
        display: flex;
        justify-content: space-between;
        margin: auto;
    }
    .input-field {
        padding: 2px 10px;
        font-size: 16px;
        color: #555;
        border-radius: 5px;
        outline: none;
        border: 1px solid #e5e5e5;
    }
</style>
