<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<EvidenceDefaultLayout {data} title="Road to Data Engineer by DataTH">
	<slot slot="content" />
</EvidenceDefaultLayout>
