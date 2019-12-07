<script context="module">
  export function preload(page) {
    return this.fetch('https://svelte-http-request.firebaseio.com/meetups.json')
		.then(res => {
			if (!res.ok) {
				throw new Error('Not OK! Error!')
			}
			return res.json();
		})
		.then(data => {
			const fetchedMeetups = [];
			for (const key in data) {
				fetchedMeetups.push({
					...data[key],
					id: key
				});
      }
      return { fetchedMeetups: fetchedMeetups.reverse() };
		})
		.catch(err => {
			error = err;
			isLoading = false;
      console.log(err);
      this.error(500, 'Couldn\'t fetch meetups!');
		});
  }
</script>

<script>
  import { createEventDispatcher, onMount, onDestroy } from 'svelte';
  import { scale } from 'svelte/transition';
  import { flip } from 'svelte/animate';
  import meetups from '../meetups-store.js';
  import MeetupItem from '../components/Meetup/MeetupItem.svelte';
  import MeetupFilter from '../components/Meetup/MeetupFilter.svelte';
  import Button from '../components/UI/Button.svelte';
  import EditMeetup from '../components/Meetup/EditMeetup.svelte';
  import LoadingSpinner from '../components/UI/LoadingSpinner.svelte';

  export let fetchedMeetups;

	let editMode;
  let editedId;
  let isLoading;
  let unsubscribe;

  const dispatch = createEventDispatcher();

  let favoritesOnly = false;

  $: filteredMeetups = favoritesOnly
    ? fetchedMeetups.filter(meetup => meetup.isFavorite)
    : fetchedMeetups;

  onMount(() => {
    meetups.setMeetups(fetchedMeetups);
    unsubscribe = meetups.subscribe(items => {
      fetchedMeetups = items;
    });
  })

  onDestroy(() => {
    if (unsubscribe) {
      unsubscribe();
    }
  });

  // onMount(() => {
  //   unsubscribe = meetups.subscribe(items => (fetchedMeetups = items));
  //   isLoading = true;
  // });

  // onDestroy(() => {
  //   if (unsubscribe) {
  //     unsubscribe();
  //   }
  // });

  function setFilter(event) {
    favoritesOnly = event.detail === 1;
  }

  function savedMeetup (event) {
		editMode = null;
		editedId = null;
	}

	function cancelEdit() {
		editMode = null;
		editedId = null;
	}

	function startEdit(event) {
		editMode = 'edit';
		editedId = event.detail;
  }

  function startAdd() {
    editMode = 'edit';
  }
</script>

<style>
	#meetups {
    width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 1rem;
  }
  #meetup-controls {
    margin: 1rem;
    display: flex;
    justify-content: space-between;
  }
  #no-meetups {
    margin: 1rem;
  }
  @media (min-width: 768px) {
    #meetups {
      grid-template-columns: repeat(2, 1fr);
    }
  }
</style>

<svelte:head>
  <title>Sapper Meetdowns</title>
</svelte:head>

{#if editMode === 'edit'}
  <EditMeetup id={editedId} on:save="{savedMeetup}" on:cancel="{cancelEdit}" />
{/if}
{#if isLoading}
  <LoadingSpinner />
{:else}
  <section id="meetup-controls">
    <MeetupFilter on:select={setFilter} />
    <Button on:click="{startAdd}">
      New Meetdown
    </Button>
  </section>
  {#if filteredMeetups.length === 0}
    <p id="no-meetups">No meetdowns found, start adding!</p>
  {/if}
  <section id="meetups">
    {#each filteredMeetups as meetup (meetup.id)}
      <div transition:scale animate:flip={{duration: 300}}>
        <MeetupItem
          id={meetup.id}
          title={meetup.title}
          subtitle={meetup.subtitle}
          address={meetup.address}
          imageUrl={meetup.imageUrl}
          description={meetup.description}
          email={meetup.contactEmail}
          isFav={meetup.isFavorite}
          on:edit={startEdit}
        />
      </div>
    {/each}
  </section>
{/if}

