<template>
    <div class="toogle-container w-auto inline-block relative top-5 right-10 sm:absolute">
        <label for="toggle_button" class="toggle__button">
            <div v-if="isActive" class="toggle__label dark:text-white dark:text-opacity-50">Dark Mode</div>
            <div v-if="! isActive" class="toggle__label">Light Mode</div>

            <input type="checkbox" id="toggle_button" v-model="checkedValue">
            <span class="toggle__switch"></span>
        </label>
    </div>
</template>
<script>
export default {
    props: {
        defaultState: {
            type: Boolean, 
            default: false
        }
    },

    data() {
        return {
            currentState: this.defaultState
        }
    },

    computed: {
        isActive() {
            return this.currentState;
        },

        checkedValue: {
            get() {
                return this.defaultState
            },
            set(newValue) {
                this.currentState = newValue;
                this.$emit('change', newValue);
            }
        }
    }
}
</script>
<style scoped>
.toggle__button {
    vertical-align: middle;
    user-select: none;
    cursor: pointer;
}
.toggle__button input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    width: 1px;
    height: 1px;
}
.toggle__button .toggle__switch {
    display:inline-block;
    height:12px;
    border-radius:6px;
    width:40px;
    background: #BFCBD9;
    box-shadow: inset 0 0 1px #BFCBD9;
    position:relative;
    margin-left: 10px;
    transition: all .25s;
}

article:not(.dark) .toggle__button .toggle__switch::after, 
article:not(.dark) .toggle__button .toggle__switch::before {
    content: "";
    position: absolute;
    display: block;
    height: 18px;
    width: 18px;
    border-radius: 50%;
    left: 0;
    top: -3px;
    transform: translateX(0);
    transition: all .25s cubic-bezier(.5, -.6, .5, 1.6);
}
    article.dark .toggle__button .toggle__switch::after, 
    article.dark .toggle__button .toggle__switch::before {
        content: "";
        position: absolute;
        display: block;
        height: 18px;
        width: 18px;
        border-radius: 50%;
        right: 0;
        top: -3px;
        transform: translateX(0);
        transition: all .25s cubic-bezier(.5, -.6, .5, 1.6);
    }
article:not(.dark) .toggle__button .toggle__switch::after {
    background: #4D4D4D;
    box-shadow: 0 0 1px #666;
}
    article.dark .toggle__button .toggle__switch::after {
        background: #4D4D4D;
        box-shadow: 0 0 1px #666;
        opacity:0;
    }
article:not(.dark) .toggle__button .toggle__switch::before {
    background: #4D4D4D;
    box-shadow: 0 0 0 3px rgba(0,0,0,0.1);
    opacity:0;
}
    article.dark .toggle__button .toggle__switch::before {
        background: #4D4D4D;
        box-shadow: 0 0 0 3px rgba(0,0,0,0.1);
        opacity:100;
    }
</style>