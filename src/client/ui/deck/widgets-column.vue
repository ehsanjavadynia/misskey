<template>
<XColumn :func="{ handler: func, title: $ts.editWidgets }" :naked="true" :column="column" :is-stacked="isStacked">
	<template #header><Fa :icon="faWindowMaximize" style="margin-right: 8px;"/>{{ column.name }}</template>

	<div class="wtdtxvec">
		<XWidgets :edit="edit" :widgets="column.widgets" @add-widget="addWidget" @remove-widget="removeWidget" @update-widget="updateWidget" @update-widgets="updateWidgets" @exit="edit = false"/>
	</div>
</XColumn>
</template>

<script lang="ts">
import { defineComponent, defineAsyncComponent } from 'vue';
import { faWindowMaximize, faTimes, faCog, faPlus } from '@fortawesome/free-solid-svg-icons';
import XWidgets from '@/components/widgets.vue';
import XColumn from './column.vue';
import { addColumnWidget, removeColumnWidget, setColumnWidgets, updateColumnWidget } from './deck-store';

export default defineComponent({
	components: {
		XColumn,
		XWidgets,
	},

	props: {
		column: {
			type: Object,
			required: true,
		},
		isStacked: {
			type: Boolean,
			required: true,
		},
	},

	data() {
		return {
			edit: false,
			faWindowMaximize, faTimes, faPlus
		};
	},

	methods: {
		addWidget(widget) {
			addColumnWidget(this.column.id, widget);
		},

		removeWidget(widget) {
			removeColumnWidget(this.column.id, widget);
		},

		updateWidget({ id, data }) {
			updateColumnWidget(this.column.id, id, data);
		},

		updateWidgets(widgets) {
			setColumnWidgets(this.column.id, widgets);
		},

		func() {
			this.edit = !this.edit;
		}
	}
});
</script>

<style lang="scss" scoped>
.wtdtxvec {
	--margin: 8px;

	padding: 0 var(--margin);

	::v-deep(._panel) {
		box-shadow: none;
	}
}
</style>
