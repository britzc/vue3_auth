<template>
	<v-container>
		<v-card>
			<v-card-title>
				<div class="text-h4">
					View Post
				</div>
			</v-card-title>
			<v-card-text>
				<div class="text-h6">
					Title
				</div>
				<div>
					{{ post.data.title }}
				</div>
				<div class="text-h6">
					Content
				</div>
				<div>
					{{ post.data.content }}
				</div>
			</v-card-text>
			<v-card-actions>
				<v-btn variant="outlined" @click="cancelPost()">
					Cancel
				</v-btn>
				<v-btn v-if="loggedIn" variant="outlined" @click="editPost()">
					Edit
				</v-btn>
			</v-card-actions>
		</v-card>
	</v-container>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'
import { useRouter } from 'vue-router'
import _ from 'lodash';

import { usePostsStore } from "@/stores/posts"
const postsStore = usePostsStore()

import { useSessionStore } from "@/stores/session"
const sessionStore = useSessionStore()

const router = useRouter()

const props = defineProps({
	id: {
		type: Number,
		default: 0
	}
})

const post = reactive({
	data: {}
})

const loggedIn = computed(() => {
	return sessionStore.getLoggedIn()
})

const editPost = () => {
	router.push({ name: "edit", params: { id: post.id } })
}

const cancelPost = () => {
	router.push({ name: "home" })
}

const loadPost = () => {
	const content = _.find(postsStore.posts, { id: props.id })
	if (content) {
		post.data = content
	}
}

loadPost()

</script>







