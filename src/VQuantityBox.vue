<template>
    <v-row dense>
        <v-col cols="5">
            <v-text-field :solo="solo" :dense="dense" :dark="dark" :outlined="outlined" type="number" v-model.number="editedItem.value" @keyup.enter="onEnter" :hint="`Total: ${total}, Count: ${count}`" persistent-hint :color="editedColor" :label="quantityLabel"></v-text-field>
        </v-col>
        <v-col cols="5">
            <v-text-field :solo="solo" :dense="dense" :dark="dark" :outlined="outlined" v-model="editedItem.description" @keyup.enter="onEnter" :color="editedColor" :label="descriptionLabel"></v-text-field>
        </v-col>
        <v-col cols="2" :align-self="dense ? 'auto' : 'center'">
            <v-btn icon @click="onEnter">
                <v-icon color="success" v-if="editedIndex === -1">mdi-plus</v-icon>
                <v-icon color="warning" v-else>mdi-sync</v-icon>
            </v-btn>
        </v-col>
        <v-col cols="12">
            <v-chip-group v-model="selected" active-class="yellow--text" column @change="onChipChange">
                <v-chip small :color="color" label close v-for="(quantity, index) in quantityArray" :key="index" @click:close="onChipClose(index)">
                    <span :class="`mr-${chipCloseMargin}`">
                        {{ quantity.value }} - {{ quantity.description }}
                    </span>
                </v-chip>
            </v-chip-group>
        </v-col>
    </v-row>
</template>
<script>
export default {
    props: {
        quantityArray: Array,
        color: {
            type: String,
            default: 'teal'
        },
        solo: {
            type: Boolean,
            default: false
        },
        dense: {
            type: Boolean,
            default: false
        },
        dark: {
            type: Boolean,
            default: false
        },
        outlined: {
            type: Boolean,
            default: false
        },
        quantityLabel: {
            type: String,
            default: 'Quantity'
        },
        descriptionLabel: {
            type: String,
            default: 'Description'
        },
        chipCloseMargin: {
            type: Number,
            default: 2
        }
    },
    data: () => ({
        editedItem: {
            value: '',
            description: '',
        },
        defaultItem: {
            value: '',
            description: '',
        },
        editedIndex: -1,
        selected: ''
    }),
    computed: {
        total() {
            return this.quantityArray.length ? this.quantityArray.map(i => i.value).reduce((a, b) => a + b) : 0
        },
        count() {
            return this.quantityArray.length ? this.quantityArray.length : 0
        },
        editedColor() {
            return this.editedIndex > -1 ? 'warning' : this.color
        }
    },
    watch: {
        'editedItem': {
            handler(val) {
                if (val) {
                    if (this.editedIndex > -1) {
                        this.$emit('on-input', { index: this.editedIndex, item: val })
                    }
                }
            },
            deep: true,
        },
    },
    methods: {
        onEnter() {
            this.editedIndex > -1 ? this.$emit('on-update', { index: this.editedIndex, item: this.editedItem }) : this.$emit('on-add', this.editedItem)
            this.resetEditedItem()
        },
        onChipClose(index) {
            index > -1 ? this.$emit('on-chip-close', index) : this.resetEditedItem()
        },
        onChipChange() {
            this.selected === undefined ? this.resetEditedItem() : (this.editedItem = Object.assign({}, this.quantityArray[this.selected]),
                this.editedIndex = this.selected)
        },
        resetEditedItem() {
            this.editedItem = Object.assign({}, this.defaultItem)
            this.editedIndex = -1
        }
    }
}
</script>