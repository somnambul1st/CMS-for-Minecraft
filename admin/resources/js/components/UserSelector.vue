<template>
    <v-autocomplete
            v-model="selected"
            :items="items"
            :loading="isLoading"
            :search-input.sync="search"
            hide-no-data
            hide-selected
            item-text="login"
            item-value="login"
            label="Выбор игрока"
            placeholder="Введите текст для поиска"
            prepend-icon="mdi-database-search"
            return-object
    ></v-autocomplete>
</template>

<script>
    import api from 'Api';

    export default {
        name: "UserSelector",
        data: function () {
            return {
                isLoading: false,
                items: [],
                selected: null,
                search: null,
            }
        },
        watch:{
            'selected': function (val) {
                console.log('Selected user:' + JSON.stringify(val));

                this.$emit('input', val);
                this.$emit('selected', val);
            },
            search (val) {
                if (this.isLoading) return;

                this.isLoading = true;

                api.get('manager/user/find?q=' + val)
                    .then(res => {
                        this.items = res.data.data;
                    })
                    .catch(err => {
                        console.log(err)
                    })
                    .finally(() => (this.isLoading = false))

            }
        }
    }
</script>