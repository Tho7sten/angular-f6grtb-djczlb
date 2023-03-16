
<script>
export default {
    data() {
        return {
            selectedItem: null,
            items: Array.from({ length: 100000 }),
            loading: false
        };
    },
    loadLazyTimeout: null,
    methods: {
        onLazyLoad(event) {
            this.loading = true;

            if (this.loadLazyTimeout) {
                clearTimeout(this.loadLazyTimeout);
            }

            //imitate delay of a backend call
            this.loadLazyTimeout = setTimeout(() => {
                const { first, last } = event;
                const _items = [...this.items];

                for (let i = first; i < last; i++) {
                    _items[i] = { label: `Stadt #${i}`, value: i };
                }

                this.items = _items;
                this.loading = false;
            }, Math.random() * 1000 + 250);
        }
    }
};
</script>

<template>
    <div class="card flex justify-content-center">
        <Dropdown v-model="selectedItem" :options="items" optionLabel="label" optionValue="value" class="w-full md:w-14rem"
            :virtualScrollerOptions="{ lazy: true, onLazyLoad: onLazyLoad, itemSize: 38, showLoader: true, loading: loading, delay: 250 }" placeholder="Hier die Auswahl" />
    </div>
    <span> Es wurde ausgewählt: {{ selectedItem }}</span>
</template>
