<template>
	<v-app>
		<video autoplay muted loop id="myVideo" v-if='((loggedIn && !admin) || !loggedIn)'>
			<source :src="formatVideoUrl" type="video/mp4">
		</video>
		<adminHeaderComponent v-if='loggedIn && admin' />
		<v-content>
			<v-container fluid>
				<div class='container' v-if='loggedIn && admin'>
					<alertComponent />
					<router-view></router-view>
				</div>
				<div class='container' v-if='loggedIn && !admin'>
					<v-card class="mx-auto">
						<v-card-text>
							<h3>Ack!</h3>
							<p>You have found the admin section. You do not have permission to be here.</p>
							
							<iframe width="100%" height="315" src="https://www.youtube.com/embed/fBGWtVOKTkM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
						</v-card-text>
					</v-card>
				</div>
				<div class='container' v-if='!loggedIn'>
					<v-card class="mx-auto" max-width="480" elevation='10'>
						<v-card-title>
							<v-icon large left>mdi-account</v-icon>
							<span class="title font-weight-light" v-if='totalUsers > 0'>Login</span>
							<span class="title font-weight-light" v-if='totalUsers === 0'>Register</span>
						</v-card-title>
						<v-card-text>
							<div class="text--primary">
								<registerComponent v-if='totalUsers === 0' />
								<loginComponent v-if='totalUsers > 0' />
							</div>
						</v-card-text>
  					</v-card>
				</div>
			</v-container>
		</v-content>
		<footerComponent v-if='loggedIn && admin' />
		<backToTopComponent />
	</v-app>
</template>


<script>
	import footerComponent from '@/layouts/footerComponent';
	import adminHeaderComponent from '@/layouts/adminHeaderComponent';
	import loginComponent from '@/components/loginComponent';
	import registerComponent from '@/components/registerComponent';
	import backToTopComponent from '@/components/backToTopComponent';
	import alertComponent from '@/components/alertComponent';
	export default {
		name: 'admin-layout-component',
		components: {
			footerComponent,
			adminHeaderComponent,
			loginComponent,
			registerComponent,
			backToTopComponent,
			alertComponent
		},
		props: [],
		data() {
			return {}
		},
		created() {
			this.$store.dispatch('counterStore/getUserCount');
		},
		computed: {
			totalUsers() {
				return this.$store.state.counterStore.users;
			},
			loggedIn() {
				return this.$store.getters['userStore/loggedIn'];
			},
			admin() {
				return this.$store.getters['userStore/admin'];
			},
			settings() {
				return this.$store.state.settingsStore.settings;
			},
			formatVideoUrl() {
				if (this.settings.backgroundVideo !== undefined || this.settings.backgroundVideo != null) {
					return '/upload/' + this.settings.backgroundVideo;
				} else {
					return '/videos/claravue_seal.mp4';
				}
			}
		},
		methods: {}
	};
</script>

<style scoped>
	#myVideo {
	  position: fixed;
	  right: 0;
	  bottom: 0;
	  min-width: 100%; 
	  min-height: 100%;
	}
</style>