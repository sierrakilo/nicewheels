<template>
    <app-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Cars</h2>
            <admin-nav></admin-nav>
        </template>
        <div class="max-w-7xl mx-auto">
            <pagination :info="cars"></pagination>
            <admin-cars :cars="cars"></admin-cars>
            <pagination :info="cars"></pagination>
        </div>
    </app-layout>
</template>

<script>
import { ChevronLeftIcon, ChevronRightIcon } from '@heroicons/vue/solid'
import AppLayout from '@/Layouts/AppLayout'
import AdminCars from '../../../Components/Admin/Cars'
import Pagination from "../../../Components/Pagination"
import AdminNav from '@/Components/Admin/AdminNav'

export default {
    name: 'AdminCarsIndex',
    components: {
        ChevronLeftIcon,
        ChevronRightIcon,

        AppLayout,
        AdminCars,
        AdminNav,
        Pagination
    },
    props: {
        cars: {
            type: Object,
            default: null
        },
        makes: {
            type: Object,
            default: null
        }
    },
    mounted () {
        this.$store.commit('admin/setSingleProperty', { key: 'makes', value: this.makes })
    }
}
</script>

<style scoped>

</style>
