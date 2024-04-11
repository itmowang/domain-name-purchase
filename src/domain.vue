<template>
    <div class="bg-green-100 min-h-screen flex justify-center ">
        <div class="container mx-auto px-4 py-8">
            <h2 class="text-2xl font-bold mb-4 text-pink-600">域名展示列表</h2>
            <input v-model="searchQuery" type="text" placeholder="搜索域名"
                class="w-full px-4 py-2 mb-4 rounded-md border border-gray-300 focus:outline-none focus:border-pink-600">
            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
                <div v-for="(domain, index) in paginatedDomains" :key="index"
                    class="border border-gray-200 rounded-md overflow-hidden shadow-md flex flex-col justify-between bg-white">
                    <div class="p-4">
                        <h3 class="text-lg font-semibold text-pink-600">{{ domain.name }}</h3>
                        <p class="text-gray-600">价格: {{ formatCurrency(domain.price) }}</p>
                    </div>
                </div>
            </div>
            <div v-if="totalPages > 1" class="mt-4 flex justify-center">
                <button @click="prevPage" :disabled="currentPage === 1"
                    :class="{ 'cursor-not-allowed opacity-50': currentPage === 1 }"
                    class="px-4 py-2 mr-2 rounded-md bg-pink-600 text-white">上一页</button>
                <button @click="nextPage" :disabled="currentPage === totalPages"
                    :class="{ 'cursor-not-allowed opacity-50': currentPage === totalPages }"
                    class="px-4 py-2 rounded-md bg-pink-600 text-white">下一页</button>
            </div>
            <div class="mt-8 text-center">
          <p>联系方式：</p>
          <p>QQ: 1531780018</p>
          <p>微信: mdzzvip</p>
          <p>邮箱: mw@loli.wang</p>
        </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            domains: [],
            searchQuery: '',
            currentPage: 1,
            pageSize: 8 // 每页显示的卡片数量
        }; 
    },
    mounted() {
        this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await fetch('/domain.json');
                this.domains = await response.json(); 
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        },
        formatCurrency(amount) {
            return new Intl.NumberFormat('zh-CN', { style: 'currency', currency: 'CNY' }).format(amount);
        },
        prevPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        }
    },
    computed: {
        paginatedDomains() {
            const startIndex = (this.currentPage - 1) * this.pageSize;
            const endIndex = startIndex + this.pageSize;
            return this.filteredDomains.slice(startIndex, endIndex);
        },
        filteredDomains() {
            return this.domains.filter(domain => {
                return domain.name.toLowerCase().includes(this.searchQuery.toLowerCase());
            });
        },
        totalPages() {
            return Math.ceil(this.filteredDomains.length / this.pageSize);
        }
    }
};
</script>

<style>
/* 这里可以添加自定义样式 */
</style>