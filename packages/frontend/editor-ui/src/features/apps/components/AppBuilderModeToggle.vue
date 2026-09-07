<script setup lang="ts">
import { N8nIconButton } from '@n8n/design-system';
import { useI18n } from '@n8n/i18n';

export type BuilderMode = 'settings' | 'preview';

const props = defineProps<{
	modelValue: BuilderMode;
}>();

const emit = defineEmits<{
	'update:modelValue': [mode: BuilderMode];
}>();

const i18n = useI18n();
</script>

<template>
	<div :class="$style.group" role="group" :aria-label="i18n.baseText('apps.page.mode.label')">
		<N8nIconButton
			icon="settings"
			:variant="props.modelValue === 'settings' ? 'subtle' : 'ghost'"
			size="small"
			:aria-pressed="props.modelValue === 'settings'"
			:aria-label="i18n.baseText('apps.page.mode.settings')"
			data-test-id="page-mode-settings"
			@click="emit('update:modelValue', 'settings')"
		/>
		<N8nIconButton
			icon="play"
			:variant="props.modelValue === 'preview' ? 'subtle' : 'ghost'"
			size="small"
			:aria-pressed="props.modelValue === 'preview'"
			:aria-label="i18n.baseText('apps.page.mode.preview')"
			data-test-id="page-mode-preview"
			@click="emit('update:modelValue', 'preview')"
		/>
	</div>
</template>

<style lang="scss" module>
.group {
	display: inline-flex;
	gap: 2px;
	border: 1px solid var(--border-color);
	border-radius: var(--radius);
	padding: 2px;
}
</style>
