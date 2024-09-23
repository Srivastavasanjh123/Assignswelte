<script>
  let services = [];  // Start with an empty list

  let newService = { name: "", description: "", price: "" };
  let isEditing = false;
  let editingIndex = null;

  // Add a new service
  function addService() {
    if (newService.name && newService.description && newService.price) {
      services = [...services, { id: Date.now(), ...newService }];
      newService = { name: "", description: "", price: "" }; // Reset form
    }
  }

  // Edit an existing service
  function editService(index) {
    isEditing = true;
    editingIndex = index;
    newService = { ...services[index] };
  }

  // Update service details
  function updateService() {
    services = services.map((service, index) =>
      index === editingIndex ? { ...newService } : service
    );
    newService = { name: "", description: "", price: "" };
    isEditing = false;
    editingIndex = null;
  }

  // Delete a service
  function deleteService(index) {
    services = services.filter((_, i) => i !== index);
  }
</script>

<h1>Healthcare Services</h1>

<!-- Form to add or update a service -->
<form on:submit|preventDefault={isEditing ? updateService : addService}>
  <input type="text" bind:value={newService.name} placeholder="Service Name" required />
  <input type="text" bind:value={newService.description} placeholder="Description" required />
  <input type="number" bind:value={newService.price} placeholder="Price" required />

  <button type="submit" class="add-service-button">{isEditing ? 'Update Service' : 'Add Service'}</button>
</form>

<!-- Display the list of services -->
<ul>
  {#each services as service, index}
    <li>
      <strong>{service.name}</strong> - {service.description} ($ {service.price})
      <button class="edit-button" on:click={() => editService(index)}>Edit</button>
      <button class="delete-button" on:click={() => deleteService(index)}>Delete</button>
    </li>
  {/each}
</ul>

<style>
  h1 {
    text-align: center;
    margin-bottom: 20px;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    margin: 10px 0;
    padding: 10px;
    background-color: #f5f5f5;
    border-radius: 5px;
  }

  form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
  }

  input {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }

  button {
    padding: 10px;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    opacity: 0.9;
  }

  /* Add service button styling */
  .add-service-button {
    background-color: #4caf50; /* Green color for add */
  }

  .add-service-button:hover {
    background-color: #45a049;
  }

  /* Edit button styling */
  .edit-button {
    background-color: #2196f3; /* Blue for edit */
  }

  .edit-button:hover {
    background-color: #1976d2;
  }

  /* Delete button styling */
  .delete-button {
    background-color: #f44336; /* Red for delete */
  }

  .delete-button:hover {
    background-color: #e53935;
  }
</style>
