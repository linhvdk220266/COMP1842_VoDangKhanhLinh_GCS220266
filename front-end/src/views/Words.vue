<template>
    <div>
        <h1>Words ( Total: {{ this.words.length }})</h1>
        <table id="words" class="ui celled center aligned compact table">
            <thead>
                <tr>
                    <th><i class="united kingdom flag"></i> English</th>
                    <th><i class="germany flag"></i> German</th>
                    <th><i class="vietnam flag"></i> Vietnamese</th>
                    <th colspan="4">Action</th>
                </tr>
            </thead>
            <tr v-for="(word, i) in words" :key="i">
                <td>{{ word.english }}</td>
                <td>{{ word.german }}</td>
                <td>{{ word.vietnamese }}</td>
                <td width="75" class="center aligned">
                    <router-link :to="{ name: 'show', params: { id: word._id } }">Show</router-link>
                </td>
                <td width="75" class="center aligned">
                    <router-link :to="{ name: 'edit', params: { id: word._id } }">Edit</router-link>
                </td>
                <td width="75" class="center aligned" @click.prevent="onDestroy(word._id)"><a
                        :href="`/word/${word._id}`">Destroy</a></td>
            </tr>
        </table>
    </div>
</template>

<script>
import { api } from '../helpers/helpers';

export default {
    name: 'words',
    data() {
        return {
            words: []
        };
    },
    methods: {
        async onDestroy(id) {
            const sure = window.confirm('Are you sure?');
            if (!sure) return;
            await api.deleteWord(id);
            this.flash('Word deleted sucessfully!', 'success');
            const newwords = this.words.filter(word => word._id !== id);
            this.words = newwords;
        }
    },
    async mounted() {
        this.words = await api.getWords();
    }
};
</script>
