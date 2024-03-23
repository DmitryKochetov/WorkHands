<template>
    <div class="calendarwrapper">
        <div class="yearWrapper">
            <div class="yearWrapper__arrowLeft" @click="previousMonth()">
                <svg width="26" height="26" viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg">
                    <path d="M6 8l4-3.5v7L6 8z" fill="#2b2b79"></path>
                </svg>
            </div>
            <div class="monthAndYearWrapper">
                {{ monthsActual[this.actualDate.getMonth()] }}
                {{ this.actualDate.getFullYear() }}
            </div>

            <div class="yearWrapper__arrowRight" @click="nextMonth()">
                <svg width="26" height="26" viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg">
                    <path d="M10 8l-4 3.5v-7L10 8z" fill="#2b2b79"></path>
                </svg>
            </div>
        </div>
        <div class="daysOfWeekWrapper">
            <div class="daysOfWeekWrapper__day" v-for="(day, index) in weeksActual" :key="index">
                {{ day }}
            </div>
        </div>
        <div class="daysWrapper">
            <div v-for="(day, index) in currentMonth" :key="index"
                :class="isSelectedElementIndex == index ? 'selectedDay' : ''"
                @click="giveMeClickedDate($event.target.innerText), selectThisDay(index, $event.target.innerText)"
                class="daysWrapper__day">
                {{ day }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CalendarComp',
    props: {
        msg: String,
        lang: String
    },
    data() {
        return {
            monthsEng: ["jan", "feb", "mar", "apr", "may", "jun", "jul", "aug", "cep", "oct", "nov", "dec"],
            monthsRu: ["янв", "фев", "мар", "апр", "май", "июн", "июл", "авг", "сен", "окт", "ноя", "дек"],
            daysEng: ["sun", "mon", "tue", "wed", "thu", "fri", "sat"],
            daysRu: ["пн", "вт", "ср", "чт", "пт", "сб", "вс"],
            daysInMonth: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
            daysInMonthLeapYear: [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
            refresh: 0,
            isSelectedElementIndex: -1,
        }
    },
    computed: {
        actualDate() {
            if (!this.msg) {
                return new Date();
            } else {
                return new Date(this.msg);
            }
        },

        langShift() {
            switch (this.lang) {
                case 'ru':
                    return 0;
                case 'eng':
                    return 1;
                default:
                    return 0;
            }
        },

        monthsActual() {
            switch (this.lang) {
                case 'ru':
                    return this.monthsRu;
                case 'eng':
                    return this.monthsEng;
                default:
                    return this.monthsRu;
            }
        },

        weeksActual() {
            switch (this.lang) {
                case 'ru':
                    return this.daysRu;
                case 'eng':
                    return this.daysEng;
                default:
                    return this.daysRu;
            }
        },

        isYearLeap() {
            let year = this.actualDate.getFullYear();
            this.refresh;
            return (year % 4 == 0 && year % 100 != 0) || year % 400 == 0;
        },

        currentMonth() {
            let currentMonthArray = [];
            this.refresh;

            if (this.isYearLeap) {
                for (let index = 1; index < (this.actualDate.getDay() + this.langShift); index++) {
                    currentMonthArray.push("");
                }
                for (let index = 1; index <= this.daysInMonthLeapYear[this.actualDate.getMonth()]; index++) {
                    currentMonthArray.push(index);
                }
            } else {
                for (let index = 1; index < (this.actualDate.getDay() + this.langShift); index++) {
                    currentMonthArray.push("");
                }
                for (let index = 1; index <= this.daysInMonth[this.actualDate.getMonth()]; index++) {
                    currentMonthArray.push(index);
                }
            }
            return currentMonthArray;
        },
    },
    methods: {
        previousMonth() {
            this.actualDate.setMonth(this.actualDate.getMonth() - 1);
            this.refresh++;
        },
        nextMonth() {
            this.actualDate.setMonth(this.actualDate.getMonth() + 1);
            this.refresh++;
        },
        giveMeClickedDate(date) {
            if (date) {
                console.log(date);
                this.actualDate.setDate(date)
                this.$emit('giveMeClickedDate', this.actualDate);
                console.log(this.actualDate);
            }
        },
        selectThisDay(dayNumber, date) {
            if (date) {
                this.isSelectedElementIndex = dayNumber;
            }
        },
    },
}
</script>

<style scoped lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    scroll-behavior: smooth;
}

.calendarwrapper {
    max-width: 25vw;
    max-height: auto;
    border: 1px solid rgb(43, 43, 121);
    border-radius: 10px;
    padding: 6px;
    color: rgb(43, 43, 121);

}

.yearWrapper {
    margin: 20px 0 20px 0;
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    align-items: center;
}

.monthAndYearWrapper {
    display: flex;
    flex-direction: row;
    align-items: center;

}

.daysOfWeekWrapper {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    margin-bottom: 20px;
}

.daysWrapper {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: repeat(5, 1fr);
    row-gap: 1vh;
    height: auto;

    &__day {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 4vh;
        border-radius: 6px;
    }

    &__day:hover {
        border: 1px solid rgb(43, 43, 121);
    }

}

.selectedDay {
    border: 1px solid rgb(43, 43, 121);
    background-color: rgb(220, 220, 255);
}
</style>