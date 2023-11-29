<template>
<div>
    <span>Multiline message is:</span>
    <p style="white-space: pre-line">{{ text_value }}</p>
    <textarea v-model="text_value" placeholder="add multiple lines"></textarea>
    <button @click="process_text">Traductor de Señas</button>
    <img :src="img_url" v-if="img_url" alt="AI Image" />
</div>
</template>

<script>
import {
    ref
} from "vue";
import fs from "fs"

const text_value = ref("");
const img_url = ref(null);

async function query(filename) {
	const data = fs.readFileSync(filename);
	const response = await fetch(
		"https://api-inference.huggingface.co/models/RavenOnur/Sign-Language",
		{
			headers: { Authorization: "Bearer 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" },
			method: "POST",
			body: data,
		}
	);
	const result = await response.json();
	return result;
}

query("cats.jpg").then((response) => {
	console.log(JSON.stringify(response));
});
</script>

<style scoped></style>
