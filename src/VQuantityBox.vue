<template>
    <v-row dense>
        <v-col cols="5">
            <v-text-field :solo="solo" :dense="dense" :dark="dark" :outlined="outlined" type="number" v-model.number="editedItem.value" @keyup.enter="onEnter" :hint="`Total: ${sum}, Count: ${quantityArraySize}`" persistent-hint :color="editedColor" :label="quantityLabel"></v-text-field>
        </v-col>
        <v-col cols="5">
            <v-text-field :solo="solo" :dense="dense" :dark="dark" :outlined="outlined" v-model="editedItem.description" @keyup.enter="onEnter" :color="editedColor" :label="descriptionLabel"></v-text-field>
        </v-col>
        <v-col cols="2">
            <v-btn icon @click="onEnter">
                <v-icon color="success" v-if="editedIndex === -1">mdi-plus</v-icon>
                <v-icon color="warning" v-else>mdi-sync</v-icon>
            </v-btn>
        </v-col>
        <v-col cols="12">
            <v-chip-group v-model="selected" active-class="yellow--text" column @change="onChipChange">
                <v-chip small :color="color" label close v-for="(quantity, index) in quantityArray" :key="index" @click:close="onChipClose(index)">
                    <span class="pr-2">
                        {{ quantity.value }} - {{ quantity.description }}
                    </span>
                </v-chip>
            </v-chip-group>
        </v-col>
    </v-row>
</template>
<script>
import _ from 'lodash'
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
        selected: '',
        lastEdited: [],
        lastEditedIndex: -1,
    }),
    computed: {
        sum() {
            if (this.quantityArray) {
                return _.toString(_.sumBy(this.quantityArray, o => o.value))
            } else {
                return 0
            }
        },
        quantityArraySize() {
            if (!_.isEmpty(this.quantityArray)) {
                return _.size(this.quantityArray)
            } else {
                return 0
            }
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
            if (this.editedIndex > -1) {
                this.$emit('on-update', { index: this.editedIndex, item: this.editedItem })
            } else {
                this.$emit('on-add', this.editedItem)
            }
            this.resetEditedItem()
        },
        onChipClose(index) {
            if (index !== -1) {
                this.lastEdited.push(this.quantityArray[index])
                this.lastEditedIndex = index
                this.$emit('on-chip-close', index)
            }
        },
        onChipChange() {
            if (!_.isUndefined(this.selected)) {
                this.editedItem = _.assign({}, this.quantityArray[this.selected])
                this.editedIndex = this.selected
            } else if (_.isUndefined(this.selected)) {
                this.resetEditedItem()
            }
        },
        resetEditedItem() {
            this.editedItem = _.assign({}, this.defaultItem)
            this.editedIndex = -1
        },
    }
}
</script>