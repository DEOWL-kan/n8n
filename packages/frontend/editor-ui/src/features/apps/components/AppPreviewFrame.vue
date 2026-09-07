<script setup lang="ts">
import { N8nIconButton, N8nSegmentControl } from '@n8n/design-system';
import { useI18n } from '@n8n/i18n';
import { computed, ref } from 'vue';

const props = defineProps<{
	pageUrl: string;
}>();

const i18n = useI18n();

type Device = 'desktop' | 'mobile';
const device = ref<Device>('desktop');
const deviceOptions = computed(() => [
	{ label: i18n.baseText('apps.page.preview.desktop'), value: 'desktop' as Device },
	{ label: i18n.baseText('apps.page.preview.mobile'), value: 'mobile' as Device },
]);

const frameWidth = computed(() => (device.value === 'mobile' ? '390px' : '100%'));

// Bumping this remounts the iframe, which is the only way to force-reload one.
const reloadKey = ref(0);
const reload = () => {
	reloadKey.value += 1;
};
</script>

<template>
	<div :class="$style.container">
		<div :class="$style.toolbar">
			<N8nSegmentControl v-model="device" :options="deviceOptions" size="small" />
			<N8nIconButton
				icon="refresh-cw"
				variant="subtle"
				size="small"
				:aria-label="i18n.baseText('apps.page.preview.reload')"
				data-test-id="page-preview-reload"
				@click="reload"
			/>
		</div>
		<div :class="$style.frame">
			<iframe
				:key="reloadKey"
				:src="props.pageUrl"
				:style="{ width: frameWidth }"
				:class="$style.iframe"
				data-test-id="page-preview-iframe"
			/>
		</div>
	</div>
</template>

<style lang="scss" module>
.container {
	display: flex;
	flex-direction: column;
	gap: var(--spacing--sm);
	width: 100%;
}

.toolbar {
	display: flex;
	align-items: center;
	justify-content: space-between;
}

.frame {
	display: flex;
	justify-content: center;
	border: 1px solid var(--border-color);
	border-radius: var(--radius--lg);
	background-color: var(--background--surface);
	padding: var(--spacing--md);
}

.iframe {
	height: 70vh;
	max-width: 100%;
	border: 0;
}
</style>
