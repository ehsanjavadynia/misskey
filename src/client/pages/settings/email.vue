<template>
<FormBase>
	<FormGroup>
		<template #label>{{ $ts.emailAddress }}</template>
		<FormLink to="/settings/email/address">
			<template v-if="$i.email && !$i.emailVerified" #icon><Fa :icon="faExclamationTriangle" style="color: var(--warn);"/></template>
			<template v-else-if="$i.email && $i.emailVerified" #icon><Fa :icon="faCheck" style="color: var(--success);"/></template>
			{{ $i.email || $ts.notSet }}
		</FormLink>
	</FormGroup>

	<FormLink to="/settings/email/notification">
		<template #icon><Fa :icon="faBell"/></template>
		{{ $ts.emailNotification }}
	</FormLink>

	<FormSwitch :value="$i.receiveAnnouncementEmail" @update:value="onChangeReceiveAnnouncementEmail">
		{{ $ts.receiveAnnouncementFromInstance }}
	</FormSwitch>
</FormBase>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faCog, faExclamationTriangle, faCheck } from '@fortawesome/free-solid-svg-icons';
import { faBell, faEnvelope } from '@fortawesome/free-regular-svg-icons';
import FormButton from '@/components/form/button.vue';
import FormLink from '@/components/form/link.vue';
import FormBase from '@/components/form/base.vue';
import FormGroup from '@/components/form/group.vue';
import FormSwitch from '@/components/form/switch.vue';
import * as os from '@/os';

export default defineComponent({
	components: {
		FormBase,
		FormLink,
		FormButton,
		FormSwitch,
		FormGroup,
	},

	emits: ['info'],
	
	data() {
		return {
			INFO: {
				title: this.$ts.email,
				icon: faEnvelope
			},
			faCog, faExclamationTriangle, faCheck, faBell
		}
	},

	mounted() {
		this.$emit('info', this.INFO);
	},

	methods: {
		onChangeReceiveAnnouncementEmail(v) {
			os.api('i/update', {
				receiveAnnouncementEmail: v
			});
		},
	}
});
</script>
