<script>
  import Modal from './Modal.svelte';

  export let message;
</script>

<Modal title="Occured, An Error!" on:cancel>
  <p>{message}</p>
</Modal>