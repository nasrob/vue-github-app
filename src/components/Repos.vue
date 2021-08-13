<template>
    <div>
        <h1>Repos</h1>
        <div v-for="repo of repos" :key="repo.id">
            <h2>{{ repo.owner.login }} / {{ repo.name }}</h2>
            <Issues :owner="repo.owner.login" :repo="repo.name" />
        </div>
    </div>
</template>
<script>
import { octokitMixin } from '@/mixins/octoMixin';
import Issues from "./repo/Issues";

export default {
    name: "Repos",
    components: {
        Issues,
    },
    data() {
        return {
            repos: [],
        };
    },
    mixins: [octokitMixin],
    async mounted() {
        const octokit = this.createOctokitClient();
        const { data: repos } = await octokit.request("/user/repos");
        this.repos = repos;
    }
}
</script>