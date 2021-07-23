<template>
    <div>
        <NavBar />
        
        <div class="container">
            <h1 class="mt-1">My Assets</h1>
            <div class="row">
                <div class="col-7">
                    <h4>List of Assets</h4>
                    <ul class="list-group custom-list-group">
                        <li class="list-group-item list-group-item-action btn-li" v-for="asset in assets" :key="asset.id" @click="onSelected(asset)">
                            {{ asset.name }} <span class="float-right">{{ asset.value }}</span>
                        </li>
                    </ul>

                </div>
                <div class="col-4">
                    <AssetView :asset="selectedAsset" @saved="onChanges" @newAsset="onNew" @deleted="onChanges"/>

                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
export default {
    data() {
        return {
            assets: [],
            selectedAsset: {}
        }
    },
    methods: {
        async getMyAssets() {
            await this.$axios.get('/api/assets')
            .then((res) => {
                if(res.status == 200) {
                    this.assets = res.data
                }
            })
        },
        onChanges() {
            this.getMyAssets()
            this.selectedAsset = {}
        },
        onSelected(asset) {
            this.selectedAsset = asset
        },
        onNew() {
            this.selectedAsset = {}
        },
    },
    created() {
        this.getMyAssets()
    }
}
</script>

<style scoped>
.btn-li {
    color: #b3b3b3;
    font-weight: 300;
    cursor: pointer;
    background-color: #25252b;
}

.btn-li:hover {
    background-color: #2e2e36;
}

.btn-li:active {
    background-color: #2c2c33;
}


</style>