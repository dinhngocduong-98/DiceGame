<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>{{titlePopup}}</h3>
            <p>{{contentPopup}}</p>
            <button 
                class="confirm"
                v-on:click="confirm"
            >
            OK
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'popup-rule',
    data() {
        return {

        }
    },
    props: {
        titlePopup: {
            type: String,
            default: 'Luat choi'
        },
        contentPopup: {
            type: String,
            default: ''
        },
        isOpenPopup: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        getClassPopup() {
            return {
                'open-popup': this.isOpenPopup
            }
        }
    },
    methods: {
        confirm() {
            this.$emit("confirmRule")
        }
    }
}
</script>

<style>
    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
        opacity: 0;
        visibility: hidden;
    }
    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
        transition: all .3s ease;
    }
    .rule {
        width: 350px;
        padding: 20px;
        top: 50%;
        left: 50%;
        position: absolute;
        background-color: #fff;
        transform: translate(-50%, -50%) scale(1.2);
        transition: all .3s ease;
    }
    .open-popup .rule {
        transform: translate(-50%, -50%) scale(1);
    }

    .rule h3 {
        margin-top: 10px;
    }

    .rule .confirm {
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border:  2px solid #333;
        background-color: #fff;
    }
    .rule .confirm:hover {
        color: #fff;
        background-color: #333;
    }

    /* kết hợp css3 với vuejs để xây dựng hiệu ứng - Zoom in Zoom out */
</style>