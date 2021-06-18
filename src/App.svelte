<script>

	let contact = {
		id: 0,
		firstName: '',
		lastName: '',
		phoneNumber: '',
		email: '',
	};

	let contacts = [{
		id: 1,
		firstName: 'John',
		lastName: 'test',
		phoneNumber: '1234',
		email: 'john@example.com',
	},{
		id: 2,
		firstName: 'Peter',
		lastName: 'test',
		phoneNumber: '1234',
		email: 'peter@example.com',
	}];

	let isEdit = false;
	let errorMessage = '';

	const handleCancel = () => {
		contact = {
			...contact,
			id: 0,
			firstName: '',
			lastName: '',
			phoneNumber: '',
			email: ''
		};

		isEdit = false;
		errorMessage = '';
	};

	const handleSubmit = () => {
		errorMessage = '';

		if(contact.firstName?.length &&
			contact.lastName?.length &&
			contact.phoneNumber?.length &&
			contact.email?.length) {

				if (contact.id === 0) {
					contact = {
						...contact,
						id: contacts.length + 1,
						firstName: contact.firstName,
						lastName: contact.lastName,
						phoneNumber: contact.phoneNumber,
						email: contact.email,
					};

					contacts = [ ...contacts, contact ];
					handleCancel();
				} else {
					contact = { ...contact };

					const newContacts = contacts.map(item => {
						if (item.id === contact.id) {
							item = contact;
						}

						return item;
					});

					contacts = [ ...newContacts ];
					handleCancel();
				}
			} else {
				errorMessage = 'Please complete the form';
			}
	};

	const handleEdit = (id) => {
		isEdit = true;
		contact = contacts.filter(contact => contact.id === id)[0];
	};

	const handleDelete = (id) => {
		const newContacts = contacts.filter(contact => contact.id !== id);
		contacts = [...newContacts];
		isEdit = false;
		handleCancel();
	};

</script>

<div class="container">
	<h1 class="header">Contacts</h1>

	<div class="inner">
		<form on:submit|preventDefault={handleSubmit}>
			<div class="form-group">
				<input type="text"
				placeholder="First Name"
				class="form-control"
				bind:value={contact.firstName}>
			</div>
			<div class="form-group">
				<input type="text"
				placeholder="Last Name"
				class="form-control"
				bind:value={contact.lastName}>
			</div>
			<div class="form-group">
				<input type="tel"
				placeholder="Phone Number"
				class="form-control"
				bind:value={contact.phoneNumber}>
			</div>
			<div class="form-group">
				<input type="email"
				placeholder="Email"
				class="form-control"
				bind:value={contact.email}>
			</div>
			<div class="form-group">

				<button type="button" class="btn btn-link" on:click={handleCancel}>
					Cancel
				</button>
				
				{#if isEdit}
					<button type="submit" class="btn btn-link">
						Update
					</button>
				{:else}
					<button type="submit" class="btn btn-link">
						Save
					</button>
				{/if}
			
			</div>
			
			{#if errorMessage.length}
				<p class="error">{errorMessage}</p>
			{/if}

		</form>
		<div class="contacts-list">
			{#each contacts as contact}
				<div class="contact-item">
					<div class="contact-item-header">
						<button class="edit btn btn-link" on:click={handleEdit(contact.id)}>Edit</button>
						<button class="delete btn btn-danger" on:click={handleDelete(contact.id)}>Delete</button>
					</div>
					<div class="contact-item-body">
						<p>{contact.firstName}</p>
						<p>{contact.lastName}</p>
						<p>{contact.phoneNumber}</p>
						<p>{contact.email}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.container {
		height: 100vh;
		padding: 5%;
	}

	.header {
		text-align: center;
	}

	.inner {
		display: flex;
		flex-direction: row;
		padding: 5%;
	}

	form {
		width: 40%;
		padding: 0 5% 0 0;
	}

	.contacts-list {
		width: 50%;
		display: flex;
		flex-wrap: wrap;
	}

	.contact-item {
		flex: 1 1 40%;
		border: 1px solid #ccc;
		border-radius: 5px;
	}

	.contact-item-header {
		display: flex;
		justify-content: flex-end;
		border-bottom: 1px solid #ccc;
		width: 100%;
		padding: 10px;
	}

	.contact-item-body {
		padding: 15px;
	}

	.error {
		color: red;
	}
</style>