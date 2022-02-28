<template>
	<div class="card">
		<a :href="manga.url" target="_blank">
			<img 
				:src="manga.images.webp.image_url" 
				:alt="manga.title + ' Poster'" 
			/>
			<h3>{{ manga.title }}</h3>
		</a>
	</div>
</template>

<script>
export default {
	props: {
		manga: {
			type: Object,
			required: true
		}
	}
}
</script>

<style lang="scss" scoped>
.card {
	flex: 1 1 32%;
	width: 32%;
	max-width: 32%;
	padding: 0px 8px;
	margin-bottom: 16px;
	img {
		width: 100%;
		height: 450px;
		object-fit: fill;
		border-radius: 16px;
		box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
		transition: 0.4s;
	}
	h3 {
		padding: 16px;
		color: #313131;
		transition: .4s;
	}
	&:hover {
		img {
			transform: scale(1.05);
		}
		h3 {
			color: #e62cf0;
		}
	}
}
</style>