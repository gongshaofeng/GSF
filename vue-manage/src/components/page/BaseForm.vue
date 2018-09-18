<template>
    <div>
        <h1>基本表单</h1>
    </div>
</template>

<script>
export default {
    data(){
        return {}
    },
    created(){
        this.fetchData()
    },
    methods:{
        fetchData: async function () {
            const res = await this.http.get(this.api.shoplist)
            console.log(res)
            if (res.data.success) {
                alert("请求成功")
            }
            }
    }
}
</script>
