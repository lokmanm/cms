<template>
    <data-list :columns="columns" :rows="rows">
        <div class="card p-0" slot-scope="{ filteredRows: rows }">
            <data-list-table :rows="rows">
                <template slot="cell-title" slot-scope="{ row: collection }">
                    <a :href="collection.entries_url">{{ collection.title }}</a>
                </template>
                <template slot="actions" slot-scope="{ row: collection, index }">
                    <dropdown-list>
                        <dropdown-item :text="__('Edit Collection')" :redirect="collection.edit_url" />
                        <dropdown-item :text="__('Scaffold Resources')" :redirect="collection.scaffold_url" />
                        <dropdown-item
                            v-if="collection.deleteable"
                            :text="__('Delete Collection')"
                            class="warning"
                            @click="$refs[`deleter_${collection.id}`].confirm()"
                        >
                            <resource-deleter
                                :ref="`deleter_${collection.id}`"
                                :resource="collection"
                                @deleted="removeRow(collection)">
                            </resource-deleter>
                        </dropdown-item>
                    </dropdown-list>
                </template>
            </data-list-table>
        </div>
    </data-list>
</template>

<script>
import Listing from '../Listing.vue'

export default {

    mixins: [Listing],

    props: [
        'initial-rows',
        'initial-columns',
    ],

    data() {
        return {
            rows: this.initialRows,
            columns: this.initialColumns
        }
    }

}
</script>
