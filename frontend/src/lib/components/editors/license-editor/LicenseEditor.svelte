<script lang="ts">
	import Collapsable from '$lib/components/basics/Collapsable.svelte'
	import type { CreateLicense } from '../../../trpcClient'
	import Labeled from '../../basics/Labeled.svelte'
	import LicenseLimitEditors from './limits/LicenseLimitEditors.svelte'

	export let entity: CreateLicense

	export let inputError: string | null = null

	$: {
		if (!entity.licenseKey) {
			inputError = 'License key can not be empty'
		}
	}
</script>

<div class="flex flex-col gap-12 xs:w-1/2">
	<div>
		<h2 class="pageSubTitle">License Key</h2>
		<input
			class="w-full mt-1 text-2xl tracking-widest border-b-2 border-b-gray-400 focus:border-b-blue-400 plain"
			type="text"
			bind:value={entity.licenseKey}
		/>
	</div>

	<Collapsable class="bg-white border">
		<svelte:fragment slot="label">
			<h2 class="pageSubTitle">License Grants</h2>
			<p>Describe the Feature this license grants or unlocks. the backend will expose this information once the license is verified</p>
		</svelte:fragment>

		<div class="flex flex-col gap-4">
			<textarea bind:value={entity.grants} />
			<p class="text-gray-500 text-sm">
				Each entry should be "name=value" entries can be separated by lines example:
				<br />
				max-tenants=10
				<br />
				advanced-dashboard=yes
			</p>
		</div>
	</Collapsable>

	<Collapsable class="bg-white border">
		<svelte:fragment slot="label">
			<h3 class="flex items-center font-medium">
			Your Notes
			</h3>

			<p>
				These notes are only visible to you. You can use them to keep track of important information
				about this license.
			</p>
		</svelte:fragment>
		<div class="flex flex-col gap-4">
			<Labeled label="Name">
				<input type="text" bind:value={entity.name} />
			</Labeled>

			<Labeled label="Notes">
				<textarea bind:value={entity.notes} />
			</Labeled>
		</div>
	</Collapsable>

	<div class="flex flex-col gap-4">
		<h2 class="pageSubTitle">Limit License</h2>

		<LicenseLimitEditors bind:entity />
	</div>
</div>
