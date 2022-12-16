<template>
	<!-- Uncomment the following component to add a form for a `modelName` -->
	<ul style="display: flex; flex-direction: column; gap: 1rem">
		<li style="border-bottom: 1px solid gray; padding-bottom: 1rem" v-for="post in posts">
			<div>
				<h3>{{ post.creator }}</h3>
				<h1>{{ post.title }}</h1>
				<h1>{{ post.body }}</h1>
			</div>
		</li>
	</ul>
</template>

<script>
import { DirectSecp256k1HdWallet, DirectSecp256k1Wallet } from '@cosmjs/proto-signing';
import { SpCrud, SpAcc } from '@starport/vue';
import SpCrudDelete from '@starport/vue/src/components/SpCrudDelete/SpCrudDelete.vue';
import { computed } from 'vue';
import { useStore } from 'vuex';
import { txClient, queryClient } from '../store/generated/blog/blog.blog/module/index';

const mnemonic =
	'gain large nerve liberty air arrest assist right oil reason obtain wire truth senior excite sell south true future rather tonight popular luxury group';
// const wallet = new DirectSecp256k1HdWallet(mnemonic, { prefix: 'blog' });
const wallet = await DirectSecp256k1HdWallet.generate(24);
console.log('wallet', wallet);
// const directSecp256k1Wallet = new DirectSecp256k1Wallet(mnemonic);
// const accounts = await wallet.getAccounts();
// console.log('wallet', accounts[0].address);

// const client = await txClient(wallet);
// console.log('client', { client });

const queryClt = await queryClient();
const posts = await queryClt.queryPosts();

export default {
	name: 'Data',

	components: { SpCrud, SpAcc, SpCrudDelete, SpCrudDelete },

	setup() {
		// store
		let $s = useStore();

		// computed
		let address = computed(() => $s.getters['common/wallet/address']);

		return {
			address,
		};
	},
	data() {
		return {
			posts: posts.data.Post,
		};
	},
};
</script>
